(credits)=
# Credits and License

You can refer to this book as:

> `<editors>` (`<year>`) _`<title>`_. `<url to book website>`. Source files at `<link to github repo`. CC BY 4.0.

You can refer to individual chapters or pages within this book as:

> `<Title of Chapter or Page>`. In `<editors>` (`<year>`) _`<title>`_. `<url to specific page on book website>`. Source files at `<link to specific commit / file in github repo`. CC BY 4.0.

## How the book is made
This website is written in markdown and jupyter notebooks files, which are converted to html using tools from [TeachBooks](https://teachbooks.io/). The files are stored on a [public GitHub repository](`<link to GitHub repo>`). The website can be viewed at `<link to book website url>`.

To recreate the website you have two options (more information in the [TeachBooks manual](https://teachbooks.io/manual/):
- In the GitHub interface: fork this repository, enable Github Pages from the source GitHub actions (Settings - Code and automation - Pages - Build and deployment - Source - GitHub Actions), enable workflows (Actions - I understand my workflows, go ahead and enable them) and run the call-deploy-book workflow (Actions - call-deploy-book - Run workflow - Run workflow). The website is released on the URL as shown on the workflow summary when the workflow has finished (Actions - call-deploy-book - call-deploy-book - Summary).
- On your own computer: clone this repository, install the required packages (`pip install -r requirements.txt`) and build the book (`teachbooks build book`). The website is stored locally in `book/_build/index.html`.

## License
This book is [CC BY 4.0 licensed](https://creativecommons.org/licenses/by/4.0/) allowing you to share and adapt the material, as long as the source is named. External resources that are reused in this book are listed below.
