---
sourceType: "{{itemType}}"{% for type, creators in creators | groupby("creatorType") -%}{% if loop.first %}
{% endif %}{{type | replace("interviewee", "author") | replace("director", "author") | replace("presenter", "author") | replace("podcaster", "author") | replace("programmer", "author") | replace("cartographer", "author") | replace("inventor", "author") | replace("sponsor", "author")  | replace("performer", "author") | replace("artist", "author")}}: "{%- for creator in creators -%}{%- if creator.name %}{{creator.name}}{%- else %}{{creator.lastName}}, {{creator.firstName}}{%- endif %}{% if not loop.last %}; {% endif %}{% endfor %}"{% if not loop.last %}
{% endif %}{%- endfor %}{% if title %}
title: "{{title}}"{% endif %}{% if publicationTitle %}
publication: "{{publicationTitle}}"{% endif %}{% if date %}
publishDate: {{date | format("YYYY-MM-DD")}}{% endif %}{% if archive %}
archive: "{{archive}}"{% endif %}{% if archiveLocation %}
archive-location: "{{archiveLocation}}"{% endif %}
annotationDate: "{{exportDate | format("YYYY-MM-DD")}}"
repokey: {{citekey}}
repo: "zotero"
---

{% if tags.length > 0 -%}{% for t in tags -%}#{% if t.tag == "secondary" %}source/secondary{% if not loop.last %}{% endif %}{% elif t.tag == "primary" %}source/primary{% if not loop.last %}{% endif %}{% elif "-project" in t.tag %}project/{{t.tag | lower | replace(" ", "-") | replace("-project", "")}}{% else %}{{t.tag | lower | replace(" ", "-")}}{% endif %}{% if not loop.last %} {% endif %}{%- endfor %}{%- endif %}


### Title

{{title}}

URI: [online-repo]({{uri}})

### Citation

{{bibliography}}

{% if abstractNote %}

### Abstract
> {{abstractNote|replace("\n"," ")}}

{% endif %}

---

### Annotations

{% for annotation in annotations %}
{% if annotation.annotatedText %}
> <mark style="background-color: {{annotation.color}};color: black">Highlight</mark> 
> {{annotation.annotatedText}}
> [Page {{annotation.page}}](zotero://open-pdf/library/items/{{annotation.attachment.itemKey}}?page={{annotation.page}})
{% endif %}
{% if annotation.comment %}
> <mark style="background-color: {{annotation.color}};color: black">Comment</mark>
> {{annotation.comment}}
> [Page {{annotation.page}}](zotero://open-pdf/library/items/{{annotation.attachment.itemKey}}?page={{annotation.page}})
{% endif %}
{% if annotation.imageRelativePath %}
![[{{annotation.imageRelativePath}}]]
{% endif %}
{% endfor %}

---

{% persist "notes" %}{% if isFirstImport %}

### Takeaways

-  

### Related

-

{% endif %}{% endpersist %}

