Before entering a command, press `Esc`. The Esc key switches Vim to **Normal** mode. Once in Normal mode, if you press `:` (make sure your keyboard is set to English layout and CapsLock is off, then press Shift + ;), a `:` symbol will appear at the bottom of the screen, and the editor will switch to **Command-Line** mode. This guarantees that you are indeed entering a command rather than editing the file. Remember that **command case matters**.

Most commands have shortened versions. Optional parts are shown in square brackets, for example: `com[mand]`.

Commands *in italics* apply only to Vim (they are not implemented in Vi).

---

#### Exit safely (will not work if there are unsaved changes)

- `:q[uit]` — exit the current Vim window. If this window is the last one, exit Vim entirely. If the current buffer has unsaved changes, this command will not work.
- *`:qa[ll]`* — close all windows and exit Vim. It will not work if there are unsaved changes in any buffer.

#### Exit with confirmation (prompts if there are unsaved changes)

- *`:conf[irm] q[uit]`* — close all windows and exit Vim, asking for confirmation if there are buffers with unsaved changes.
- *`:conf[irm] xa[ll]`* — save all changes, close all windows, and exit Vim, asking for confirmation if any buffers cannot be saved.

#### Write (save) changes and exit

- `:wq` — write (save) the current buffer to its file (even if there were no changes) and close the window.
- *`:wqa[ll]`* — do the same for all windows.
- `:wq!` — same as above, but also writes files marked as read-only.
- *`:wqa[ll]!`* — the same, for all windows.
- `:x[it]`, `ZZ` (but with certain [nuances](http://vimdoc.sourceforge.net/htmldoc/editing.html#ZZ)) — save the file only if there are changes and exit.
- *`:xa[ll]`* — do the same for all windows.

#### Discard changes and exit

- `:q[uit]!`, *`ZQ`* — exit without saving, including cases where the visible buffers have unsaved changes. This will not work if there are unsaved changes in hidden buffers.
- *`:qa[ll]!`*, *`:quita[ll][!]`* — exit without saving, discarding all changes in both visible and hidden buffers.

Press Enter to execute the entered command.
