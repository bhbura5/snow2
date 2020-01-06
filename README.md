# ServiceNow Snippets

## Table of Contents
@todo fix ToC, not sure about JavaScript snippets and Misc
1. [Service Portal](docs/service_portal.md)
1. [JavaScript Snippets](docs/javascript.md)
1. [HTML Snippets](docs/html_snippets.md)
1. [SASS Variables](docs/sass_variables.md)
1. [CSS](docs/css.md)
1. [Misc](docs/misc.md)
1. [Components](docs/components/components.md)
1. [Widget Generate Catalog](docs/widget_generate_catalog.md)

## Template

----

## Snippet Title
---
### Description

```js
// code
```

> Notes on the snippet

----

## Snippets

---

### Filtering choices lists (Use reference qualifier)

![alt text](https://i.imgur.com/gfBXiYs.png)

```js
javascript:"u_contractISNOTEMPTY^u_contract =" + current.variables.contract

current.variables.contract => get value the variable on the form
u_contractISNOTEMPTY^ => condition u_contract não deve estar vazio
```

Keywords: `Use_reference_qualifier`, `filter_choice_list`

---

### Verifying a record is found using `GlideRecord.get`
```js
var gr = new GlideRecord(table);
if(gr.get(sys_id)) {
  // it is found
  gs.print(gr.sys_created_on);
} else {
  // it isn't found
  gs.print('record not found');
}
```
Keywords: `GlideRecord`, `get`

----

### Using `gs.nil()`
```js
// @todo
if(gs.nil(gr.variableName)) { }
```
> `gs.nil()` will return true if a variable is empty

----

### Force a record into an update set
```js
var rec = new GlideRecord('table_name_of_record');
rec.get('sys_id_of_record');
//Push the record into the current update set   
var um = new GlideUpdateManager2();
um.saveRecord(rec);
```
---
### GlideRecord Update Example

```js 
  var gr = new GlideRecord('sys_user');
  gr.addQuery('emailLIKE@domain');
  gr.query();
  var count = gr.getRowCount();
  if (count > 0) {
    while (gr.next()) {
      gr.user_password.setDisplayValue('convit123');
      gr.password_needs_reset = true;
      gr.update();
    }
  }
```

> Notes on the snippet
Resume: that code update password of all users on the filter
Caution: User gr.setWorkflow = false for skipp workflow rules
tags: GlideREcord, Update
----

### GlideRecord Get Example

```js
// var gr = new GlideRecord('sys_db_object');
  gr.addQuery('super_class.labelSTARTSWITHtask^nameSTARTSWITHu_');
  gr.query();
  var count = gr.getRowCount();
  if (count > 0) {
    while (gr.next()) {

      gs.addInfoMessage(gr.name + ' - ' + tableHasData(gr.name))
      
    }
  }


function tableHasData(table){
   var rec = new GlideRecord(table); 
   rec.query(); 
   return rec.hasNext()
}


```

> Notes on the snippet
Resume: that code return all tables extended from task and if has records or not on that 
extendables tables

### Recursive function
```js
var results = [];
var nestedCategories = ['898fc5a0db00d74074c99447db9619d8'];
getChildren(nestedCategories[0]);
searchItems();
return results;

function getChildren(sysID) {
    var gr = new GlideRecord('sc_category');
    gr.addQuery('parent', sysID);
    gr.addActiveQuery();
    gr.query();
    while(gr.next()) {
        var current = gr.sys_id.toString();
        nestedCategories.push(current);
        if(hasChildren(current)) {
            getChildren(current);
        }
    }
}

function hasChildren(sysID) {
    var gr = new GlideRecord('sc_category');
    gr.addQuery('parent', sysID);
    gr.addActiveQuery();
    gr.query();
    if(gr.next()) {
        return true;
    } else {
        return false;
    }
}
```

----

### CatItem API
```js
var catalogItemJS = new sn_sc.CatItem(sc.getUniqueValue());
if (!catalogItemJS.canView())
    continue;
var catItemDetails = catalogItemJS.getItemSummary();
```

----

### CatCategory API
```js
categoryJS = new sn_sc.CatCategory(data.category_id);
if (!categoryJS.canView()) {
    data.error = gs.getMessage("You do not have permission to see this category");
    return;
}
```

----

### CatalogSearch API
```js
var items = data.items = [];
var catalog = $sp.getValue('sc_catalog');
var sc = new sn_sc.CatalogSearch().search(catalog, data.category_id, '', false, options.depth_search);
sc.addQuery('sys_class_name', 'NOT IN', 'sc_cat_item_wizard');
if (data.keywords)
    sc.addQuery('123TEXTQUERY321', data.keywords);
sc.orderBy('order');
sc.orderBy('name');
sc.query();
```

----

### Get Days Ago
```js
_checkDaysAgo: function (date) {
    //take date to find today, yesterday, etc.
    var now = gs.now() + ' 12:00:00';
    date = date.substring(0, 10) + ' 12:00:00';

    var nowDT = new GlideDateTime();
    nowDT.setDisplayValue(now);
    var dateDT = new GlideDateTime();
    dateDT.setDisplayValue(date);
    var seconds = gs.dateDiff(dateDT.getDisplayValue(), nowDT.getDisplayValue(), true);

    var days = seconds / 60 / 60 / 24;

    return days;
}
```

---

### Working with GlideRecord.getRefRecord()
@todo - found in nr-fav-panel-list widget
```js
var articleRef = gr.u_knowledge_article.getRefRecord();
item.title = articleRef.getDisplayValue('short_description') + " (" + gr.getDisplayValue('u_knowledge_article') + ")";
```


---

### Working with Dates


#### Comparing if a Date is Less Than Current Date
```js
var date = new GlideDateTime(date_field);
var now = new GlideDateTime();
if(date <= now) {
    // date is in the past
}
```


---
