# Search
For even simpler browsing of notes, Codice provides search facility on every page.
Just click on the loupe icon to reveal the form. Same effect can be achieved by
using <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>F</kbd> shortcut.

### Search by phrase
In fact, there is nothing to explain. Just type in the phrase you are looking for
and press enter (or click on the button).

### Filtering
Another advanced functionallity built right into Codice and the reason why this
documentation page even exists is *filtering*. It allows to construct the query
narrowing the search results using specified criteria.

<div class="alert alert-info">
Currently, it is not possible to combine filters with searched phrase. If correct
syntax, from the table below, is used, search will turn into filtering mode.
</div>

<table class="table">
    <tr>
        <th>Keyword</th>
        <th>Allowed values</th>
        <th>Description</th>
        <th>Example(s)</th>
    </tr>
    <tr>
        <td><code>date</code></td>
        <td>Date in <code>DD.MM.YYYY</code> or <code>MM/DD/YYYY</code> format</td>
        <td>Filters notes by their creation dates</td>
        <td><code>date:11.01.2017</code>, <code>date:03/11/2017</code></td>
    </tr>
    <tr>
        <td><code>status</code></td>
        <td><code>done</code>, <code>undone</code>, <code>pending</code>, <code>expired</code></td>
        <td>Filters notes by their status</td>
        <td><code>status:done</code>, <code>status:pending</code></td>
    </tr>
</table>

All keywords are separated from their values using a colon (<code>:</code>).

Additionally, it is possible to use negation operator, placing <code>!</code>
in front of the value. So for example, to return all notes with status other
than done `status:!done` can be supplied.
