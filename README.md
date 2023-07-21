# infobase

My Notes and Writeups

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
- in mkdocs.yml, all location is in reference to base dir (i.e. mastersite). set site_dir to point to root/public (gitlab needs html assets here), set docs_dir to root/content folders (where .md files/dir stored)
-  no page/md file in repo outside the source docs_dir can be referenced in link
-  mind the indentation for subkeys in mkdocs.yml (e.g. as in toc/permalink)


### Blogib netlify
Main gatsby settings in gatsby-config.js:
- for source files, point to the /content/blog folder (in relative to base dir sitebuild/blogsite set in netlify cicd deploy settings). I.e. path: `../../content/blog`  (in source-file plugin)
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
- then you can reset pwd.  (set partha gmail / emni dflt)

Note:
- admin folder (for cms) is kept in base dir/static/ dir for gatsby
- in case


## Limitations
- wikilink (mkdoc better)
  - gatsby not able to link wikilink images. but normal image markdown ref working  (for md file wikilink ref working fine)
  - mkdocs able to link both types 
- source dir (gatsby better)
  - gatsby can take both md and imgage from anywhere in repo (outside of base dir and even if md, image in separate dir)
  - while mkdocs can take source directory outside of base dir tree, but if attachments (referred from md files) are outside of base dir tree then it will not work. Hence for note, entire content directory taken as source and exclude the blog/etc folders not needed in mkdocs build. However this has a side effect of notebook folder is shown in first level instead of its content (so a redirect is conifigured in netlify.toml to redirect from root in published site)  