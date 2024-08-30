# Auto-Check-Permissions-Js

## Overview

The `Auto-Check-Permissions` repository provides a JavaScript utility class that helps developers manage and enforce user permissions on the client side. This tool is useful for dynamically adjusting the UI based on the permissions assigned to a user, such as enabling or disabling buttons, hiding elements, or restricting access to certain actions.

## Features

- **Dynamic Permission Checking:** Automatically checks user permissions and updates the UI accordingly.
- **Element Control:** Disables, hides, or modifies elements based on the user's permissions.
- **Reusable Code:** Easily integrate the class into any project that requires dynamic permission handling.
- **Customizable Actions:** Supports custom actions for hiding or disabling elements based on specific conditions.


```javascript

class CheckPermissionsClass {

    get_permissions() {
        const permissionsContainer = document.getElementById('permissions-container-in-layout');
        const permissionsString = permissionsContainer.dataset.permissions
        const permissionsArray = JSON.parse(permissionsString)
        return permissionsArray;
    }
    checkpermissions(PermissionObjects) {
        var permissions_result = {}
        const permissions = this.get_permissions();
        for (const PermissionObject of PermissionObjects) {
            const { actionclass, permission } = PermissionObject;
            const fields = document.getElementsByClassName(actionclass);
            permissions_result[permission] = true
            if (!permissions.includes(permission)) {
                for (let i = 0; i < fields.length; i++) {
                    const field = fields[i];
                    field.disabled = true;
                    field.style.display = 'none';
                    permissions_result[permission] = false
                }
            }
        }
        return permissions_result
    }

    hiddenactioncolumn(class_name, update_permission, delete_permission) {
        if (update_permission == false && delete_permission == false) {
            const fields = document.getElementsByClassName(class_name);
            for (let i = 0; i < fields.length; i++) {
                const field = fields[i];
                field.disabled = true;
                field.style.display = 'none';
            }
        }
    }

    hiddeneditactioncolumn(class_name, update_permission) {
        if (update_permission == false) {
            const fields = document.getElementsByClassName(class_name);
            for (let i = 0; i < fields.length; i++) {
                const field = fields[i];
                field.disabled = true;
                field.style.display = 'none';
            }
        }
    }

    hiddenstatuscolumn(class_name, update_permission) {
        if (update_permission == false) {
            const fields = document.getElementsByClassName(class_name);
            for (let i = 0; i < fields.length; i++) {
                const field = fields[i];
                field.classList.add("disabled");
                field.removeAttribute('onclick');
            }
        }
    }

}

```

# Usage

## Example Usage

To use the `CheckPermissionsClass` in your project, follow these steps:

- Define Permissions in Your HTML:

Ensure that your HTML has a container that stores the user's permissions in a data-permissions attribute as a JSON string.

```javascript
<span class="add permission-add">
  <a href="javascript:void(0)" data-bs-toggle="modal"  data-bs-target="#add"><img
      src="images/add_icon.svg" alt="" /> Add New</a>
</span>
```


- Create Permission Objects:

In your JavaScript, define an array of permission objects where each object includes the class of the elements to control and the corresponding permission required.

```javascript

const PermissionObjects = [
    {
        "actionclass": "permission-add",
        "permission": "Create"
    },
    {
        "actionclass": "permission-edit",
        "permission": "Update"
    }
];

```

- Instantiate and Use the Class:

Instantiate the CheckPermissionsClass and call the checkpermissions method, passing in the permission objects.


```javascript
$(document).ready(function() {
    const checkpermissionsobject = new CheckPermissionsClass();
    checkpermissionsobject.checkpermissions(PermissionObjects);
});

```


- Hide or Disable Action Columns:

Use the hiddenactioncolumn, hiddeneditactioncolumn, and hiddenstatuscolumn methods to further customize the visibility of elements based on specific conditions.


```javascript
$(document).ready(function() {
    const checkpermissionsobject = new CheckPermissionsClass();
    let permissions_result = checkpermissionsobject.checkpermissions(PermissionObjects);
    const Update = permissions_result["Update"];
    const Delete = permissions_result["Delete"];
    checkpermissionsobject.hiddenactioncolumn("permission-action", Update, Delete);
});
```


```javascript
$(document).ready(function() {
    const checkpermissionsobject = new CheckPermissionsClass();
    let permissions_result = checkpermissionsobject.checkpermissions(PermissionObjects);
    const Update = permissions_result["Update"];
    checkpermissionsobject.hiddeneditactioncolumn("permission-action", Update);
});
```

```javascript
$(document).ready(function() {
    const checkpermissionsobject = new CheckPermissionsClass();
    let permissions_result = checkpermissionsobject.checkpermissions(PermissionObjects);
    const Update = permissions_result["Update"];
    checkpermissionsobject.hiddenstatuscolumn("permission-status", Update);
});
```

## Example


```javascript
<body>
  <span class="add permission-add">
    <a href="javascript:void(0)" data-bs-toggle="modal"  data-bs-target="#add"><img src="images/add_icon.svg" alt="" /> Add New</a>
  </span>

  <table id="main-table" class="main-table">
      <thead>
        <tr>
          <th></th>
          ...
          <th class="permission-status"></th>
          <th class="permission-edit">Action</th>
        </tr>
      </thead>
      <tbody>
          <tr>
            <td></td>
            ...
            <td class="permission-status">Active</td>
            <td class="permission-edit">
                <span class="edit mx-2"
                  ><a href=""><img src="{% static 'images/edit.svg' %}" alt="" /></a
                ></span>
              </td>
          </tr>
      </tbody>
  </table>
</body>
<script>
  $(document).ready(function() {
     const PermissionObjects = [
        {
            "actionclass": "permission-add",
            "permission": "Create"
        },
        {
            "actionclass": "permission-edit",
            "permission": "Update"
        }
    ];
      const checkpermissionsobject = new CheckPermissionsClass();
      let permissions_result = checkpermissionsobject.checkpermissions(PermissionObjects);
      const Update = permissions_result["Update"];
      const Delete = permissions_result["Delete"];
      checkpermissionsobject.hiddenactioncolumn("permission-action", Update, Delete);
      checkpermissionsobject.hiddeneditactioncolumn("permission-action", Update);
      checkpermissionsobject.hiddenstatuscolumn("permission-status", Update);
  });
</script>

```
