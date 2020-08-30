# About

[These](http://sendtoaryansh.gitbook.io/) are my informatics notes, written primarily following the language conventions of C++11 \(and thus compatible with versions greater than or equal to 11\). For the corresponding backend, visit [the GitHub page](https://github.com/Aryansh-S/Informatics-Notes). 

Chapter 1 \(Programming Contests\): If in search of more resources, I direct you to [my GitHub USACO repository](https://github.com/Aryansh-S/USACO) or [USACO Guide](https://usaco-guide.vercel.app/).

Chapter 2 \(Artificial Intelligence\): [This](https://l.messenger.com/l.php?u=https%3A%2F%2Fwww.youtube.com%2Fplaylist%3Flist%3DPLZHQObOWTQDNU6R1_67000Dx_ZCJB-3pi&h=AT1bo8LSfxw901Jzgq4rjqZCPKHLf4KfQXx21bUSUhqESkI1XcfFrcYM6OZ0D2NxbaAeTIZrkFLeGHAmAOjH3STkK02QjppgbnirSxNVWcX2937mXIin2PBjg--b82BtHqFbUTxCJ4_rGqBGb92p3vBP8RA) short video series is the best resource to get the gist of artificial intelligence from a broader standpoint. 

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

