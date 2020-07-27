# About

[These](http://sendtoaryansh.gitbook.io/) are my informatics notes, written primarily in C++11. For the corresponding backend, visit [the GitHub page](https://github.com/Aryansh-S/Informatics-Notes). If in search of more resources, I direct you to [my GitHub USACO repository](https://github.com/Aryansh-S/USACO) or [USACO Guide](https://usaco-guide.netlify.app/). 

## Convert to PDF

You might want to convert these notes into PDF format for reference. The process is a bit involved. 

You can use the following procedure with homebrew and git. 

### Prerequisites

You need npm \(via node\) to get the converter library and calibre to convert to pdf. Run the below in order.

```text
$ brew install node
$ brew link node
$ brew cask install calibre
$ npm install repo-to-pdf
```

Use git to clone the backend repository responsible for maintaining this GitBook. 

```bash
$ git clone https://github.com/Aryansh-S/Informatics-Notes
```

### Compile to HTML

Run just this one command, replacing the `<yourname>` part with your user directory on your computer. 

```bash
$ npx repo-to-pdf /Users/<yourname>/Informatics-Notes -c /Applications/calibre.app/Contents/MacOS/ebook-convert 
```

### Print HTML to PDF

This, when done, will create a file called `Informatics-Notes.html` under your user directory. Open this in a browser for instance and then print it to PDF file format, generating your PDF copy. 

### Cleanup

Remove the HTML file as well as the repository folder. 

```bash
$ rm -rf Informatics-Notes
$ rm Informatics-Notes.html
```

