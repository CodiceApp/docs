# The basics

When you log in to the Codice for the first time, you should see a screen very similiar to this:

![Main screen](assets/images/main-screen.png)

We'll shortly describe each part it composes of the GUI.

### Navigation
![Navigation](assets/images/navbar.png)

This is of course main menu for the application. Be aware that clicking on the logo brings
you to the home screen we are just describing.

### Quick form note
![Quick form for adding a note](assets/images/quickform.png)

Fastest way to add a short note. Clicking on that area opens full form (you can hide it
back using an arrow on the bottom). Type your note in, add labels or define deadline if
necessary and save. You can even use <kbd>Ctrl</kbd>+<kbd>S</kbd> while being focused on the content input.

If you are in label view (showing all notes with given label), quickform will have this
label preselected for your convenience.

### Note
![Single note](assets/images/note.png)

Notes are the essence of Codice. Single note is the most basic unit of information. You can 
assign expiration time (deadline) to the note to treat it like a task.
Categorizing notes is possible using labels (call it *tags* if you prefer) - you will learn more soon.

<div class="alert alert-danger">
Once again, there is no distinction between "note" and "task" from the Codice point of view - note
becomes a task as soon as you set deadline for it.
</div>

<div class="alert alert-info">
Note can be marked as done even if doesn't have specified time of expiration.
</div>

![Note status indicator](assets/images/status-indicator.png)

This little coloured bar is a *status indicator* for a given note. Each color means what follows:
- **green** - note (task) is done
- **blue** - task is pending
- **orange** - task is pending *but* less than 24 hours left
- **red** - deadline for a task is over
- no color - this note is not a task, no expiration time was set

![Note labels](assets/images/labels-area.png)

Top right corner of the note is a place where all labels assigned to it are shown. When creating
a label, you can set a colour for it, so it's even easier to recognize different labels with just
a quick glance.

![Note footer](assets/images/note-footer.png)

This is a note footer - it shows when the note was added and its expiration time (if it's set).
Click on the timestamp to enter detailed note view.

Hovering a note reveals additional set of buttons on the right. You can mark note as done
(or reverse this process), edit and remove a note.
