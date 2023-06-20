# infobase help

Details of infobase project & setup guides. 

## Infobase Structure

```
infobase                // git project root
|-- content/            // source note vault (root for obsidian or other md editors)
	├── .obsidian/      // obsidian config
|   |__ attachments/    // central place for all images/pdfs referred from 
|   |__ blog/           // content for publishing in blog site
|   |__ notebook/       // reference notes collected as research
|   |__ workbook/       // place for work projects
|   |   |__ temp/       // default place for new note. later to organize.
|   |   |__ ...
|__ sitebuild/           // config & code for static sites build
|   |__ blogsite/       // takes source from blog & build public blog site (gatsby, netlify)
|   |__ notesite/       // takes source from notbook & build public note site (mkdocs, netlify)
|   |__ mastersite/     // takes source from entire content folder & build private master site (mkdocs, gitlab)
|   |__ 
├── .gitlab-ci.yml      // for mastersite cicd build (kept here instead of in mastersite since gitlab needs in repo root)
├── .gitignore          // those not to commit in git       
└── README              // for git repo specific info (not used). 
```



## Installation Steps

  

### Obsidian setup

  

1. Obsidian setup

  

### Gitlab Setup

  

1. git remote setup

2. git local setup

  

Note:

- the repo is not required for local (pc) build. All build (cicd) done in respective site servers

- .gitignore file should include .obsidian so that not stored in remote.

- In case local build is done, then .gitignore must include the build assets to ignore

  

```

git init

git add --all

git commit -m "initializing"

  

git remote add origin git@gitlab.com:parthaspace/infobase.git

git remote -v

git branch --set-upstream-to=origin/main

git branch --all

  

git pull origin main

    // (if error for repointing remote: git pull origin main --allow-unrelated-histories)

git push -uf origin main

    // (for next onwards just use git pull / git push)

```

### Mastersite - Mkdocs Gitlab Site (private)

No manual settings needed (all settings in git files in sitebuild -> mastersite and gitlab-ci file in root)

  

Note for major config:

- in gitlab-ci, change base dir to where mkdocs config files reside: cd ./sitebuild/mastersite/

- in mkdocs.yml, all location is in reference to base dir (i.e. mastersite). set site_dir (i.e. build dir) to point to root/public (gitlab needs all built assets like html css etc here), set docs_dir (i.e. source dir) to root/content folders (where .md files/dir stored)

-  no page/md file/image etc in repo residing outside the source docs_dir child-tree can be referenced in link from any file

-  mind the indentation for subkeys in mkdocs.yml (e.g. as in toc/permalink)

  
  

### Blogib netlify

Main gatsby settings in gatsby-config.js:

- for source files, point to the /content/blog folder (in relative to base dir sitebuild/blogsite set in netlify cicd deploy settings). I.e. path: `../../content/blog`  (in source-file plugin)

- also ensure other paths are in relative to base dir blogsite

  

FYI:: All Netlify settings are in netlify.toml in git. No manual setup needed.

In case needed from UI, then follow below:

- Create new site in netlify with (from existing project) with below settings:

  - gitlab parthaspace repo as backend

  - base dir: sitebuild/blogsite

  - Publish dir: sitebuild/blogsite/public

  - build command: npm run build

  

**For Netlify CMS**


The cms is attached to blogib gatsby site, however the entire content directory is accessed for change. Since it changes only in gitlab so all sites auto-rebuild.

To access the CMS page, put url: <gatsby blog url>/admin (i.e. blogib.netlify.app/admin). It can be logged in by user given access through Netlify identity.

FYI:: In git:

- static/admin folder is kept under blogsite for cms attached to gatsby project.

- In config.yml, all directory paths are relative to the Repo Root. Refer to the git

- No separate setting needed. In case of folder struct change above needs to be updated.  

  

Under Netlify UI -> Site settings -> Identity :

- Services: Enable Git Gateway. Edit to fetch the token from gitlab. (some times it may expire and object error can come. In that case need to refetch token and save)

- Registration: Invite only (in mail template, unclick need for mail confirmation to avoid overhead)

Under Netlify UI -> Dashboard -> Identity tab :

- enable identity

- send invite user gmail.

- to access the CMS, in browser url, put blogib.netlify.app/admin .

- It will ask for login pwd. Need to reset password first time.

- From mail click reset link.

- Issue: However clicking reset link it appends recovery token in main url which does not work. In url, before recovery token, put admin/

- then you can reset pwd.  (set partha gmail / emni dflt)

  

Note:

- admin folder (for cms) is kept in base dir/static/ dir for gatsby
- in case
  

## Limitations

- wikilink (mkdoc better)
	- gatsby not able to link wikilink images (see issue below). but normal image markdown ref working  (for md file reference from other file, wikilink ref working fine)
	- mkdocs able to link both types (images and md file as wikilink ref)

- source dir (gatsby better)
	- gatsby can take both md and imgage from anywhere in repo (outside of base dir tree and even if md, image in separate dir)
	- while mkdocs can take source directory/files outside of base dir tree, but if attachments (referred from md files) are outside of base dir tree then it will not work. 

## Things to remember 

	### Infobase Config: 
	- Content is the Obsidian vault root. Infobase is the root and may contain assets which are not visible in obsidian to keep clean editing and also to some assets reference from outside git. 
	- Due to mkdoc source_dir limitation, for notesite (built with mkdoc), entire content directory taken as source and excluded the blog/etc folders not needed in mkdocs build for notesite. However this has a side effect of notebook folder is shown in first level instead of its immediate content (so a redirect is conifigured in netlify.toml to redirect from root in published site)
	### Content Edit: 
	- Due to Gtasby wikilink reference limitation for images, For blogsite (built with gatsby), images/attachments must be referred with standard markdown not as wikilink (however another note/md file can be referred as wikilink). 
	- gitlab has 100mb per project repo limit. Hence store images/pdf etc as much as possible outside of infobase git repo (i.e. outside content/attachment folder). < Need to decide !! >


## Outstadnding issue to look for fix:
- Issue1: Gatsby cannot translate wikilink for images (md file works fine with the plugin). Need to check for fix so that wikilink can be used for blogsite


