# Schedule List Page Scope Implementation Guide

## Overview

This guide explains how to convert your Vue.js schedule list component to work with Node-RED Dashboard 2.0 using **page scope** instead of group scope. The implementation provides a structured approach with different screen areas for better organization.

## Key Features

### 1. **Page Scope Structure**
- **Single Page**: All components are organized under one page scope
- **Multiple Groups**: Different screen areas are separated into logical groups
- **Global Templates**: Reusable template components across the page

### 2. **Screen Areas**

#### **Schedule List Area** (8 columns)
- Main schedule list display
- Handles all line types (Empty, Order, List, End)
- Interactive buttons for each line type
- Real-time updates

#### **Control Buttons** (4 columns)
- Quick action buttons
- Status summary
- System controls

#### **Machine Status** (6 columns)
- Machine status display
- Individual machine controls
- Real-time machine monitoring

#### **History & Logs** (6 columns)
- Activity history
- Filterable logs
- Action tracking

## Implementation Details

### 1. **Template Structure**

Each template uses **page scope** with the following structure:

```javascript
// Template scope is set to "local" for page-level access
templateScope: "local"
```

### 2. **Data Flow**

```
Schedule Data Generator → Multiple Templates
                     ↓
                [Schedule List]
                [Control Panel]
                [Machine Status]
                [History Logs]
```

### 3. **Action Processing**

All actions are processed through dedicated function nodes:
- `action-processor` - Handles schedule list actions
- `control-processor` - Handles control panel actions
- `machine-processor` - Handles machine control actions
- `history-processor` - Handles history panel actions

## Usage Instructions

### 1. **Import the Flow**

1. Open Node-RED editor
2. Click Menu (☰) → Import
3. Select `schedule-list-page-scope.json`
4. Deploy the flow

### 2. **Access the Dashboard**

Navigate to: `http://localhost:1880/schedule`

### 3. **Understanding the Layout**

The dashboard is organized into four main areas:

#### **Schedule List Area**
- Displays all schedule lines
- Each line type has different styling and actions
- Click on lines to interact with them

#### **Control Buttons Area**
- Quick actions for system control
- Status summary showing totals
- Emergency controls

#### **Machine Status Area**
- Real-time machine status
- Individual machine controls
- Progress monitoring

#### **History Area**
- Activity logs with timestamps
- Filterable by type (All, Actions, Errors)
- Clear history functionality

## Customization

### 1. **Adding New Line Types**

To add a new line type, modify the `schedule-data-processor` function:

```javascript
LineDataX: {
    LineType: 4, // New line type
    ScheduleListKeyID: 3001,
    // Add your custom properties
    CustomProperty: "value"
}
```

Then update the template to handle the new line type:

```html
<!-- Add to the template -->
<div v-else-if="lineData.LineType === 4" class="line-item custom-line">
    <!-- Your custom line content -->
</div>
```

### 2. **Modifying Screen Areas**

To change the layout, modify the group configurations:

```json
{
    "id": "ui-group-schedule-list",
    "width": "6",  // Change from 8 to 6 columns
    "order": 1
}
```

### 3. **Adding New Actions**

To add new actions, modify the appropriate processor function:

```javascript
case 'newaction':
    msg.payload = {
        success: true,
        message: 'New action processed',
        action: action.action
    };
    break;
```

## Data Structure

### **Schedule Line Data**

```javascript
{
    LineType: 0|1|2|3,  // 0=Empty, 1=Order, 2=List, 3=End
    ScheduleListKeyID: number,
    // Order line properties
    OrderNo: "string",
    OrderKeyID: number,
    OrderDate: "string",
    // List line properties
    ProductCode: "string",
    Length: "string",
    Qty: number,
    DoneQty: number,
    LineStatus: "string"
}
```

### **Machine Data**

```javascript
{
    id: "string",
    name: "string",
    status: "running|stopped|paused|error",
    statusText: "string",
    currentJob: "string",
    progress: number,
    uptime: "string"
}
```

### **History Entry**

```javascript
{
    type: "info|warning|error|success",
    message: "string",
    details: "string",
    timestamp: "ISO string"
}
```

## Styling

### **CSS Classes**

The templates include comprehensive CSS styling:

- `.schedule-container` - Main container
- `.line-item` - Individual line styling
- `.order-line`, `.list-line`, `.end-line` - Line type specific styles
- `.action-btn` - Button styling
- `.machine-card` - Machine status cards
- `.history-entry` - History log entries

### **Responsive Design**

The layout is responsive and adapts to different screen sizes:

```css
.machine-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 15px;
}
```

## Integration with External Systems

### 1. **Database Integration**

To connect to a database, modify the data generator:

```javascript
// Replace the sample data generation with database queries
var scheduleData = await queryDatabase('SELECT * FROM schedule_lines');
```

### 2. **API Integration**

To integrate with external APIs:

```javascript
// Add HTTP request nodes
var apiResponse = await httpRequest('GET', 'https://api.example.com/schedule');
```

### 3. **Real-time Updates**

To enable real-time updates:

```javascript
// Add MQTT or WebSocket nodes for real-time data
var realtimeData = await mqttSubscribe('schedule/updates');
```

## Best Practices

### 1. **Performance**
- Use pagination for large datasets
- Implement data caching
- Optimize template rendering

### 2. **User Experience**
- Provide loading indicators
- Implement error handling
- Add confirmation dialogs for destructive actions

### 3. **Maintenance**
- Use consistent naming conventions
- Document custom functions
- Implement proper logging

## Troubleshooting

### **Common Issues**

1. **Templates not updating**
   - Check if `storeOutMessages` is set to `true`
   - Verify `fwdInMessages` is set to `true`

2. **Actions not working**
   - Check function node connections
   - Verify action names match case-sensitively

3. **Styling issues**
   - Check CSS class names
   - Verify template scope settings

### **Debug Tips**

1. **Use Debug Nodes**
   - Add debug nodes to see message flow
   - Check payload structure

2. **Console Logging**
   - Add `node.log()` statements in functions
   - Check Node-RED console for errors

3. **Template Debugging**
   - Use browser developer tools
   - Check for JavaScript errors

## Next Steps

1. **Customize the templates** for your specific needs
2. **Integrate with your data sources** (database, APIs)
3. **Add authentication** if required
4. **Implement real-time updates** for live data
5. **Add more sophisticated error handling**
6. **Create mobile-responsive versions**

This implementation provides a solid foundation for a production schedule management system using Node-RED Dashboard 2.0 with page scope organization.