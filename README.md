<table align="center"><tr><td align="center" width="9999">
<img src="./media/images/template/ivc_logo.png" align="center" width="100" alt="Institute Logo">

# IVC Reveal JS Template

<img alt="Preview" src="./preview.png" align="center" width="600" />

</td></tr></table>

This repository contains a template for presentations at the IVC institute using the [Reveal JS](https://revealjs.com/) framework, which is a web-based alternative to presentation slides.
The benefit of having a web-based presentation is the cross plattform compatibility.
While this is already possible with a simple PDF, Reveal JS can easily handle videos and it is more elegant to host the presentation on a domain and thus simply share a link instead of a 100 MB presentation file.
A live demo can be accessed [here](https://presentations.rakuschek.at/template/).


**Attention** Basic HTML and CSS knowledge is required to work with this template and framework.

### Usage

To view the presentation, open the index.html file with any browser of your choice (please don't use browsers from the stone age era, e.g. Internet Explorer).

Presentations made with [Reveal JS](https://revealjs.com/) are merely a website.
Thus, editing the presentation is done by writing raw HTML code.
To keep this Readme concise, please refer to the [Reveal JS](https://revealjs.com/) documentation for further information.

Important: This template uses [Tailwind CSS](https://tailwindcss.com/) as a CSS framework.
The benefit of Tailwind is that it alleviates one of the hardest things in programming: Naming things.
More concisely, Tailwind provides each CSS attribute as a class of its own, such that no more classes have to be named and defined.
Tailwind is a matter of taste, so use whatever CSS framework best fits your needs.

### Exporting to PDF

Reveal JS presentations can be saved as a PDF file by using the browsers PDF printing feature.
Simply append `?print-pdf` to the end of the URL and reload the page.
This creates a view that can be directly saved to a PDF file.
Unfortunately Firefox does not work well for this task, therefore it is advisable to use Chrome or Opera for saving the PDF.