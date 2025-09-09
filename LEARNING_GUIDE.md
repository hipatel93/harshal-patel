# Node-RED and Dashboard 2.0 Learning Guide

## Table of Contents
1. [Introduction to Node-RED](#introduction-to-node-red)
2. [Getting Started](#getting-started)
3. [Core Concepts](#core-concepts)
4. [Dashboard 2.0 Basics](#dashboard-20-basics)
5. [Practical Examples](#practical-examples)
6. [Advanced Topics](#advanced-topics)
7. [Best Practices](#best-practices)

## Introduction to Node-RED

Node-RED is a flow-based development tool for visual programming, originally developed by IBM for wiring together hardware devices, APIs, and online services. It's particularly popular in IoT (Internet of Things) applications.

### Key Features:
- **Visual Programming**: Drag-and-drop interface for creating flows
- **Event-driven**: Based on messages flowing between nodes
- **Extensible**: Large ecosystem of community-contributed nodes
- **Lightweight**: Runs on resource-constrained devices
- **Web-based**: Access and edit flows through a web browser

## Getting Started

### Installation
```bash
# Install Node-RED globally
npm install -g node-red

# Or install locally in your project
npm install node-red

# Install Dashboard 2.0
npm install node-red-dashboard
```

### Running Node-RED
```bash
# Start Node-RED
node-red

# Access the editor at http://localhost:1880
```

## Core Concepts

### 1. Flows
- **Flow**: A collection of connected nodes that process messages
- **Subflow**: A reusable group of nodes
- **Tab**: Organizes multiple flows

### 2. Nodes
- **Input Nodes**: Generate messages (inject, mqtt in, http in)
- **Function Nodes**: Process messages with JavaScript
- **Output Nodes**: Send messages (debug, mqtt out, http response)

### 3. Messages
- **msg.payload**: The main data content
- **msg.topic**: Optional topic/identifier
- **msg.timestamp**: When the message was created
- **msg._msgid**: Unique message identifier

### 4. Context
- **Node Context**: Data specific to a node instance
- **Flow Context**: Data shared within a flow
- **Global Context**: Data shared across all flows

## Dashboard 2.0 Basics

Dashboard 2.0 (node-red-dashboard) provides a modern web-based UI for Node-RED applications.

### Key Components:

#### Layout
- **Tabs**: Organize different pages
- **Groups**: Group related controls
- **Base**: Base template for consistent styling

#### Input Controls
- **Button**: Trigger actions
- **Switch**: Toggle on/off
- **Slider**: Numeric input with range
- **Text Input**: Text entry
- **Dropdown**: Select from options
- **Date Picker**: Date/time selection

#### Display Elements
- **Text**: Display text content
- **Gauge**: Show numeric values
- **Chart**: Plot data over time
- **Template**: Custom HTML content
- **Notification**: Pop-up messages

#### Advanced Components
- **Colour Picker**: Color selection
- **Form**: Multi-field input
- **Spacer**: Layout spacing
- **Link**: Navigation links

## Practical Examples

### Example 1: Simple Button and Display
```json
[
  {
    "id": "button-flow",
    "type": "tab",
    "label": "Button Example",
    "disabled": false,
    "info": "Simple button to display interaction"
  },
  {
    "id": "button-node",
    "type": "ui-button",
    "z": "button-flow",
    "name": "Click Me",
    "group": "display-group",
    "order": 1,
    "width": 0,
    "height": 0,
    "passthru": false,
    "label": "Click Me",
    "tooltip": "",
    "color": "",
    "bgcolor": "",
    "className": "",
    "icon": "",
    "payload": "Hello from Node-RED!",
    "payloadType": "str",
    "topic": "button-click",
    "topicType": "str",
    "x": 200,
    "y": 100,
    "wires": [["display-node"]]
  },
  {
    "id": "display-node",
    "type": "ui-text",
    "z": "button-flow",
    "group": "display-group",
    "order": 2,
    "width": 0,
    "height": 0,
    "name": "Display",
    "label": "Message:",
    "format": "{{msg.payload}}",
    "layout": "row-spread",
    "font": "",
    "fontSize": 14,
    "color": "",
    "className": "",
    "x": 400,
    "y": 100,
    "wires": []
  }
]
```

### Example 2: Temperature Monitoring
This example shows how to create a temperature monitoring system with:
- Simulated temperature sensor
- Gauge display
- Chart for historical data
- Alert when temperature exceeds threshold

### Example 3: Smart Home Control
A more complex example featuring:
- Multiple device controls (lights, fan, temperature)
- Status displays
- Scheduling functionality
- Mobile-responsive design

## Advanced Topics

### 1. Custom CSS Styling
```css
/* Custom styles for dashboard */
.ui-dashboard .ui-page {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
}

.ui-dashboard .ui-control {
    border-radius: 10px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}
```

### 2. JavaScript Functions
```javascript
// Process incoming data
var temperature = msg.payload;
var timestamp = new Date().toISOString();

// Add status based on temperature
if (temperature > 30) {
    msg.status = "Hot";
    msg.color = "red";
} else if (temperature < 10) {
    msg.status = "Cold";
    msg.color = "blue";
} else {
    msg.status = "Normal";
    msg.color = "green";
}

// Format for display
msg.payload = {
    value: temperature,
    status: msg.status,
    timestamp: timestamp
};

return msg;
```

### 3. HTTP API Integration
```javascript
// Make HTTP request to external API
var options = {
    method: 'GET',
    url: 'https://api.weather.com/v1/current',
    headers: {
        'Authorization': 'Bearer ' + global.get('weather_api_key')
    }
};

msg.options = options;
return msg;
```

### 4. Database Integration
- **SQLite**: For local data storage
- **MySQL/PostgreSQL**: For larger applications
- **InfluxDB**: For time-series data
- **MongoDB**: For document storage

## Best Practices

### 1. Flow Organization
- Use descriptive names for nodes and flows
- Group related functionality in subflows
- Add comments and documentation
- Use consistent naming conventions

### 2. Error Handling
```javascript
// Always handle errors in function nodes
try {
    var result = JSON.parse(msg.payload);
    msg.payload = result;
} catch (error) {
    node.error("JSON parse error: " + error.message);
    msg.payload = { error: "Invalid JSON" };
}
return msg;
```

### 3. Performance Optimization
- Use appropriate node types for the task
- Avoid unnecessary function nodes
- Implement proper message filtering
- Use context variables for frequently accessed data

### 4. Security Considerations
- Validate all inputs
- Use HTTPS for external communications
- Implement proper authentication
- Keep dependencies updated

### 5. Testing and Debugging
- Use the debug node extensively
- Test with various input scenarios
- Implement logging for production
- Use version control for flows

## Next Steps

1. **Practice**: Start with simple flows and gradually increase complexity
2. **Explore**: Try different node types and configurations
3. **Build**: Create real-world projects
4. **Share**: Contribute to the Node-RED community
5. **Learn**: Stay updated with new features and best practices

## Resources

- [Node-RED Official Documentation](https://nodered.org/docs/)
- [Dashboard 2.0 Documentation](https://flows.nodered.org/node/node-red-dashboard)
- [Node-RED Community](https://discourse.nodered.org/)
- [Flow Library](https://flows.nodered.org/)
- [Node-RED Cookbook](https://cookbook.nodered.org/)

Happy learning and building with Node-RED and Dashboard 2.0!