# Delete Empty Layers

Delete all empty layers in document regardless of visibility, given an original script they wanted to have modified to include hidden layers (but instead rewritten from scratch)

> [By request on r/AdobeIllustrator](https://www.reddit.com/r/AdobeIllustrator/comments/125agta/help_with_script/)

```js
Array.prototype.forEach = function (callback) {
  for (var i = 0; i < this.length; i++) callback(this[i], i, this);
};
Array.prototype.filter = function (callback) {
  var filtered = [];
  for (var i = 0; i < this.length; i++)
    if (callback(this[i], i, this)) filtered.push(this[i]);
  return filtered;
};
function get(type, parent, deep) {
  if (arguments.length == 1 || !parent) {
    parent = app.activeDocument;
    deep = true;
  }
  var result = [];
  if (!parent[type]) return result;
  for (var i = 0; i < parent[type].length; i++) {
    result.push(parent[type][i]);
    if (parent[type][i][type] && deep)
      result = [].concat(result, get(type, parent[type][i], deep));
  }
  return result;
}

get("layers")
  .filter(function (layer) {
    return !layer.pageItems.length;
  })
  .forEach(function (layer) {
    layer.visible = true;
    layer.remove();
  });
```
