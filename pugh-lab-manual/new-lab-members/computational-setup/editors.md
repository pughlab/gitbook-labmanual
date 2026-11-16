---
description: Choosing an editor
---

# Editors

## Vim and Nano&#x20;

some of the most popular text editors. You can choose whichever editor you are most comfortable with from the many that are available. However the editor must be installed on the cluster for you to use. You might be able to ask the system admin to install it for you. Below we bring a brief intro to the two popular ones that are already available on the servers for you to use.

### Vim

There are plenty of resources online that discuss the relative merits of vim vs other text editors. The crucial aspect is that Vim is a modal editor in that it has several modes, and the behaviour of the keys is dependent on which mode you are currently in! For example pressing ‘G’ while in command mode takes to the last line in the document, while in insert mode it writes the letter ‘G’. If you are completely new to Vim, run vimtutor and go though lessons one to six. In addition, try the Introduction and Tutorial here: http://blog.interlinked.org/tutorials/vim\_tutorial.html and use the Cheat Sheet here: http://tnerual.eriogerg.free.fr/vimqrc.pdf.

### Nano

Nano is an ncurses-based editor (which means it must be run from a terminal window) that focuses on simplicity. Nano uses very simple key combinations in order to work with files. A file is either opened or started with the command:

```
nano filename
```

When you have the file open in Nano you will notice, at the bottom of the terminal window, a short list of command key-combination examples. All key combinations for Nano start with the CTRL key. To execute a command you hold the CTRL (commonly referred to as the “Control Key”) key down and click the secondary key to perform the action. The most common key combinations for Nano are:

* CTRL-x – Exits the editor. If you are in the middle of editing a file the exit process will ask you if you want to save your work.
* CTRL-r – Read a file into your current working file. This enables you to add text from another file while working from within a new file.
* CTRL-c – Display the current cursor position.
* CTRL-k – Cut text.
* CTRL-u – Uncut (or Paste) text.
* CTRL-o – Save file name and continue working.
* CTRL-t – check the spelling of your text.
* CTRL-w – Search your text.
* CTRL-a – Go to the beginning of your current working line.
* CTRL-e – Go to the end of the current working line.
* CTRL-g – Get help with Nano.
