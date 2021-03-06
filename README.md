# Configurable object format for Quill

## Installation

Just require the package using bower:

```
bower install megawebmaster/taggableObjectQuillFormat --save
```

## Usage

1st step: Require the `TaggableObjectQuillFormat` in your main module.

2nd step: Set it up during configuration phase:

    app.config((TaggableObjectQuillFormatProvider) ->
        TaggableObjectQuillFormatProvider.on('search', ['$q', ($q) ->
            (text) ->
                object =
                    id: 0
                    label: text
                promise = $q.defer()
                promise.resolve(object)
                return promise.promise
        ])
    )

3rd step: Inject `TaggableObjectQuillFormat` into controller you want format to work.

4th step: Register the format:

    app.controller('MyCtrl', ['TaggableObjectQuillFormat', (TaggableObjectQuillFormat) ->
      TaggableObjectQuillFormat.register()
    ])
    
5th step: Profit.

### Available events

You can add your own services for these events:

* `search` - fired when a object formatting button is clicked, should return a promise that resolves into the object (or the object itself) you want to add to the text
* `node.add` - fired when node is being added to the text, after DOM element creation, before adding to text, you can format the node according to values in the object (resolved in `search`)
* `node.remove` - fired when node is being removed from the document (i.e. when text being decorated is removed completely), you need to remove every attribute you have added to the node during `node.add` phase
* `node.value` - fired when node value is fetched (i.e. when you copy tagged text), needs to return a value for the tagged object that can be used to create new object from scratch

## Templating

To use Quill in your templates you need to prepare a toolbar and use `ng-quill-editor` directive, i.e.:
    
    <div id="quill-toolbar" class="toolbar toolbar-container">
        <span class="ql-format-group">
            <span title="Bold" class="ql-format-button ql-bold">
                <i class="fa fa-bold"></i>
            </span>
            <span title="Italic" class="ql-format-button ql-italic">
                <i class="fa fa-italic"></i>
            </span>
            <span title="Underline" class="ql-format-button ql-underline">
                <i class="fa fa-underline"></i>
            </span>
            <span title="Strikethrough" class="ql-format-button ql-strike">
                <i class="fa fa-strikethrough"></i>
            </span>
        </span>
        <span class="ql-format-group">
            <span title="Numbered list" class="ql-format-button ql-list">
                <i class="fa fa-list-ol"></i>
            </span>
            <span title="Bullet list" class="ql-format-button ql-bullet">
                <i class="fa fa-list-ul"></i>
            </span>
        </span>
        <span class="ql-format-group">
            <span title="Add object" class="ql-format-button ql-object">
                <i class="fa fa-plus"></i>
            </span>
        </span>
    </div>
    <ng-quill-editor theme="base" ng-model="document"></ng-quill-editor>

