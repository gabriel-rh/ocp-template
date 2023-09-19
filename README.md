# ocp-template

Use this template to create a bare bones repo for asciibinder, based oon how OCP works.

There are 4 folders needed for generating the website - these need ot live in the repo, but you can ignore them:
- **_images** contains images used in the website itself (not in the content)
- **_javascripts** has Javascript files for the website, such as the version drop-down
- **_stylesheets** contains CSS files
- **_templates** has ERB template files that determine the structure of the pages iin the website, including header, footer, left-hand sidebar nav, etc

There is a single content file **welcome/index.adoc**.

The topic map file contains a single entry for the content file:

```
---
Name: About
Dir: welcome
Distros: openshift-enterprise
Topics:
- Name: Welcome
  File: index
```


Asciibinder, the custom Ruby static site generator, uses file-based routing, where each directory in the topic map represents a node in the navigation while every file is a leaf on the tree.


The **Distros:** field must correspond to an entry in the **_distro_map.yml** file. Each "distro" is a variant of the content (or a separate "product" built from the same repo content).


To build the sample template install the asciibiinder gem and run:
```
asciibinder build --distro=openshift-enterprise
```

The generated output is in the **_preview** directory in **_preview/welcome/index.html** - open the file iin a browser to see a local build of the website.


The Python files (and **aura.tar.gz**) are used for transforming the content into a format that is suitable for the portal:

```
python3 build_for_portal.py --distro openshift-enterprise --product "OpenShift Container Platform" --version 4.13 --no-upstream-fetch
```

This generate **master.adoc** in the **drupal-build/openshift-enterprise/welcome** folder, and sets up any dependencies (images, modules, snippets, etc).

Running **makeBuild.py** generates an XML file, and is used to ensure all IDs are unique across the books when migrating the content to the portal.

```
python3 makeBuild.py
```

The XML file is output to **drupal-build/openshift-enterprise/welcome/build/master.xml**.

