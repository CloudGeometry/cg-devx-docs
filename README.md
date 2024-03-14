This project uses [MkDocs](https://www.mkdocs.org/)

To install MkDocs and required plugins run 

```shell
pip install mkdocs mkdocs-mermaid2-plugin mkdocs-drawio-file
```

```shell

.
├── README.md
├── docs
│   ├── ...
│   └── ...
└── mkdocs.yml

```
mkdocs.yml contains project configuration file. 
Documentation source files are located in a folder named `docs`. 
Documentation entry point is `index.md` file.

To run in a local dev-server mode, run the mkdocs serve command:

```shell
mkdocs serve
```

Documentation website should be available at http://127.0.0.1:8000/

To build a static website run build documentation command: 

```shell
mkdocs build
```
This output will be generated to the directory named `site`.

To host using [GitHub Pages](https://pages.github.com/) and deploy to a branch `gh-pages`, run the following command: 

```shell
mkdocs gh-deploy
```
