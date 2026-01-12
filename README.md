# Layer 8 UI Components

A collection of reusable UI components for building web applications in the Layer 8 ecosystem. These components provide consistent styling, behavior, and integration patterns that serve as the foundation for any application built on Layer 8.

## Components

### Base Components

#### Popup (`popup/`)
A generic modal/popup component with support for:
- Nested modals with stacking
- Configurable headers, content, and footers
- Tab navigation within popups
- Form data collection
- Iframe communication via postMessage
- Keyboard accessibility (Escape to close)
- Customizable sizes (small, medium, large)

```javascript
ProblerPopup.show({
    title: 'Edit Item',
    content: '<form>...</form>',
    size: 'medium',
    onSave: (formData) => { /* handle save */ }
});
```

#### Confirm (`confirm/`)
A confirmation dialog component built on top of Popup:
- Three types: `danger`, `warning`, `info`
- Customizable messages and button text
- Callback support for confirm/cancel actions
- Iframe communication support

```javascript
ProblerConfirm.danger('Delete Item', 'Are you sure you want to delete this item?', () => {
    // handle deletion
});
```

#### Table (`table/`)
A feature-rich table component (L8Table) for invoking Layer 8 API for any model:
- Client-side and server-side pagination
- Column sorting (ascending/descending)
- Column filtering with debounced input
- Customizable columns with render functions
- Action buttons (Edit, Delete, Toggle state)
- Add button support
- Enum value filtering for server-side queries
- Status tags and badges
- L8Query integration for server-side operations

```javascript
const table = new L8Table({
    containerId: 'my-table',
    columns: [
        { key: 'name', label: 'Name' },
        { key: 'status', label: 'Status', render: (item) => L8Table.statusTag(item.isUp) }
    ],
    onEdit: (id) => { /* handle edit */ },
    onDelete: (id) => { /* handle delete */ },
    serverSide: true,
    endpoint: '/api/query',
    modelName: 'MyModel'
});
table.init();
```

#### View Table (`view_table/`)
A read-only table component for displaying data from any Layer 8 model:
- Pagination with page range display
- Sorting and filtering
- Status column with colored badges
- Row click handlers
- Server-side L8Query integration

#### Edit Table (`edit_table/`)
An editable table variant with full CRUD support for Layer 8 models.

### Application Components

These are complete, ready-to-use application modules that any Layer 8 application can integrate:

#### Login (`login/`)
Authentication component with:
- Username/password authentication
- Two-Factor Authentication (TFA) with TOTP support
- QR code generation for authenticator app setup
- "Remember me" functionality
- Configurable session timeout

#### Register (`register/`)
User registration component for new user signup.

#### Users (`users/`)
User management component providing:
- Create, edit, and delete user accounts
- Assign multiple roles to users
- Password management
- Real-time role assignment display

#### Roles (`roles/`)
Role and permission management component:
- Define roles with unique identifiers
- Create authorization rules per role
- Configure per-action permissions (GET, POST, PUT, PATCH, DELETE, ALL)
- Element-type based access control
- Allow/Deny rule logic

#### Credentials (`credentials/`)
Credential management component for storing and managing authentication credentials.

#### Targets (`targets/`)
Target/endpoint management component for configuring connection targets.

## Project Structure

```
l8ui-components/
├── go/
│   ├── ui/web/
│   │   ├── popup/           # Popup component
│   │   │   ├── popup.js
│   │   │   ├── popup.css
│   │   │   ├── popup-forms.css
│   │   │   └── popup-content.css
│   │   │
│   │   ├── confirm/         # Confirmation dialog
│   │   │   ├── confirm.js
│   │   │   └── confirm.css
│   │   │
│   │   ├── table/           # L8Table component
│   │   │   ├── table.js
│   │   │   └── table.css
│   │   │
│   │   ├── view_table/      # Read-only table component
│   │   │   ├── table.js
│   │   │   └── table.css
│   │   │
│   │   ├── edit_table/      # Editable table component
│   │   │   ├── table.js
│   │   │   └── table.css
│   │   │
│   │   ├── login/           # Login component
│   │   ├── register/        # Registration component
│   │   ├── users/           # User management component
│   │   ├── roles/           # Role management component
│   │   ├── credentials/     # Credentials component
│   │   ├── targets/         # Targets component
│   │   │
│   │   ├── app.js           # App coordinator
│   │   └── styles.css       # Global styles
│   │
│   └── tests/               # Go test infrastructure
│
├── README.md
├── LICENSE
└── .gitignore
```

## Usage

### Including Components

Include the required CSS and JavaScript files in your HTML:

```html
<!-- Core styles -->
<link rel="stylesheet" href="styles.css">

<!-- Popup component -->
<link rel="stylesheet" href="popup/popup.css">
<link rel="stylesheet" href="popup/popup-forms.css">
<script src="popup/popup.js"></script>

<!-- Confirm component (requires Popup) -->
<link rel="stylesheet" href="confirm/confirm.css">
<script src="confirm/confirm.js"></script>

<!-- Table component -->
<link rel="stylesheet" href="table/table.css">
<script src="table/table.js"></script>
```

### Popup Root Container

For the Popup component, add a root container to your HTML:

```html
<div id="probler-popup-root"></div>
```

### Server-Side Integration

The table components support server-side pagination with L8Query syntax:

```javascript
const table = new L8Table({
    containerId: 'data-table',
    columns: [...],
    serverSide: true,
    endpoint: '/api/query',
    modelName: 'Device',
    baseWhereClause: 'type=router',
    transformData: (item) => ({
        id: item.id,
        name: item.name,
        status: item.status
    })
});
```

## Styling

Components use CSS custom properties for theming. Override these in your stylesheet:

```css
:root {
    --l8-primary-color: #4a90d9;
    --l8-danger-color: #e53e3e;
    --l8-warning-color: #dd6b20;
    --l8-success-color: #38a169;
}
```

## Dependencies

These components are designed to work with the Layer 8 ecosystem:
- `l8bus` - Event bus and overlay networking
- `l8web` - REST server implementation
- `l8types` - Type definitions

## Browser Support

- Chrome (recommended)
- Firefox
- Safari
- Edge

## License

This project is part of the Layer 8 Ecosystem.

## Contributing

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

## Support

For issues and feature requests, please use the GitHub issue tracker.
