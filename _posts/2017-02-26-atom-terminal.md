---
title: "Configuring terminal usage in Atom"
tags: linux atom terminal-fusion coffee-script simplicity
---

I re-visited the [Atom](https://atom.io) editor in the last weeks; after all [its performance is being improved step by step](https://github.com/atom/atom/releases/tag/v1.14.0)[^1] and by now the development speed seems to have settled down a little.

Overall, I am quite impressed with how simple the editor is to configure[^2].

For example, I ran into a problem when configuring terminal support. While the [terminal-fusion](https://atom.io/packages/terminal-fusion) package (a Linux-only fork of  [platformio-atom-ide-terminal](https://github.com/platformio/platformio-atom-ide-terminal)) works well out-of-the-box, it seems to miss a simple way to switch between terminal window and the current editor pane (and to auto-hide the terminal if it is not in focus).

Luckily, there is a [code snippet for `platformio-atom-ide-terminal`](https://github.com/platformio/platformio-atom-ide-terminal/issues/62#issuecomment-252450925)[^3] that just needs a little tinkering to make it compatible with `terminal-fusion`, and then goes into `init.coffee`:

{% highlight coffee %}
atom.packages.onDidActivatePackage (pack) ->
  if pack.name == 'terminal-fusion'
    atom.commands.add 'atom-workspace',
      'editor:focus-main', ->
        p = atom.workspace.getActivePane()
        panels = atom.workspace.getBottomPanels()
        term = panels.find (pan) ->
          pan.item.constructor.name == 'TerminalFusionView'
        if not term
          # Open a new terminal
          editor = atom.workspace.getActiveTextEditor()
          atom.commands.dispatch(atom.views.getView(editor), 'terminal-fusion:new')
        else if term and p.focused
          term.item.open()
          term.item.focus()
        else if term and !p.focused
          term.hide()
          p.activate()
{% endhighlight %}

It's easy to understand what is going on here, even without knowing much about [CoffeeScript](http://coffeescript.org/).

Now just add this shortcut definition to your `keymap.cson`:

{% highlight yaml %}
'.platform-linux, atom-text-editor atom-workspace':
  'ctrl-ä': 'editor:focus-main'
{% endhighlight %}

And yes, this is an `ä` you see there -- change as appropriate if you are not blessed with a German keyboard layout :-)


---
[^1]: My impression so far: absolutely usable for 'normal' documents (e.g. code), but still _much_ slower than [Sublime Text](https://www.sublimetext.com) on larger files (e.g. log files of a few megabytes, or files with very long lines -- although that seems to get fixed in [v1.15](https://github.com/atom/atom/releases/tag/v1.15.0-beta3)). Also, starting the editor takes a few seconds, so it's a little too slow for quick one-off edit tasks (git commits etc.).

[^2]: Of course it helps that important shortcuts like `ctrl-shift-p` are consistent with Sublime Text :-)

[^3]: The last version of that snippet, on which the above code is based,  [can be found here](https://github.com/platformio/platformio-atom-ide-terminal/issues/62#issuecomment-263085500).
