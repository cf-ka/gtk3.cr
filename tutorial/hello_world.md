# GTK3 Hello World from scratch

First create the crystal app skeleton using:

```
crystal init app helloworld
```

This will create a helloworld directory with some files.

Then add the gtk3 shard to your shard.yml:

```YAML
name: helloworld
version: 0.1.0

targets:
  helloworld:
    main: src/helloworld.cr

dependencies:
  gtk3:
    github: phil294/gtk3.cr

crystal: 1.4.1

license: MIT
```

Run `shards install` to download the dependencies, then run `bin/gi-crystal` to generate the bindings. Be sure to have the
GTK and GObject Introspection packages installed beforehand.

**Note**: You need to execute `bin/gi-crystal` every time some dependency that may affect the generated bindings change, like
the gi-crystal shard or newer versions of GTK3 library.

Now it's time to write the infamous hello world. Write the following to `src/helloworld.cr`:

```Crystal
require "gtk3"

app = Gtk::Application.new("hello.example.com", Gio::ApplicationFlags::None)
count = 0

app.activate_signal.connect do
  window = Gtk::ApplicationWindow.new(app)
  window.title = "Hello World!"
  window.set_default_size(200, 200)

  button_box = Gtk::ButtonBox.new

  button = Gtk::Button.new_with_label("Hello!!")
  button.clicked_signal.connect do
    count += 1
    button.label = "You clicked #{count} times!"
  end

  window.add(button_box)
  button_box.add(button)
  window.show_all
end

exit(app.run)
```

Now compile it normally, i.e. `shards build`, then run the program `./bin/helloworld`.

And here the fastest tutorial ever finishes. Good luck!
