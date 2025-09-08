# Getting Started with Node-RED and Dashboard 2.0

## Quick Start Guide

### 1. Installation

```bash
# Install dependencies
npm install

# Start Node-RED
npm start
```

### 2. Access Node-RED

Open your browser and go to: `http://localhost:1880`

### 3. Import Example Flows

1. In Node-RED, click the **Menu** (☰) in the top-right corner
2. Select **Import**
3. Choose one of the flow files from the `flows/` directory:
   - `basic-examples.json` - Basic Node-RED concepts
   - `dashboard-examples.json` - Dashboard 2.0 components
   - `advanced-examples.json` - Advanced features with charts and API integration
   - `smart-home-project.json` - Complete smart home automation project

### 4. Access the Dashboard

After importing dashboard flows, access the dashboard at:
- Basic Dashboard: `http://localhost:1880/dashboard`
- Advanced Dashboard: `http://localhost:1880/advanced`
- Smart Home Dashboard: `http://localhost:1880/smarthome`

## Learning Path

### Step 1: Basic Concepts (basic-examples.json)
- **Inject Node**: Generates messages
- **Debug Node**: Displays message content
- **Function Node**: Processes messages with JavaScript
- **Switch Node**: Routes messages based on conditions
- **Change Node**: Modifies message properties

### Step 2: Dashboard Components (dashboard-examples.json)
- **UI Button**: Interactive buttons
- **UI Switch**: Toggle controls
- **UI Slider**: Numeric input with range
- **UI Text**: Display text content
- **UI Gauge**: Show numeric values
- **UI LED**: Visual status indicators
- **UI Template**: Custom HTML content

### Step 3: Advanced Features (advanced-examples.json)
- **UI Chart**: Data visualization
- **UI Form**: Multi-field input forms
- **HTTP Request**: API integration
- **Data Processing**: Complex data manipulation
- **Real-time Updates**: Live data streaming

### Step 4: Complete Project (smart-home-project.json)
- **Multi-tab Dashboard**: Organized interface
- **State Management**: Persistent data storage
- **System Integration**: Multiple components working together
- **Real-time Monitoring**: Live data updates
- **User Configuration**: Settings management

## Key Concepts to Learn

### 1. Message Flow
- Messages flow from left to right through connected nodes
- Each message has a `payload` (main data) and optional `topic`
- Messages can be modified, filtered, or routed

### 2. Context Storage
- **Node Context**: Data specific to a node instance
- **Flow Context**: Data shared within a flow
- **Global Context**: Data shared across all flows

### 3. Dashboard Layout
- **Base**: Main dashboard container
- **Tab**: Organizes different pages
- **Group**: Groups related controls
- **Components**: Individual UI elements

### 4. JavaScript Functions
- Use JavaScript in Function nodes to process data
- Access message properties: `msg.payload`, `msg.topic`
- Store data in context: `context.set()`, `context.get()`
- Global storage: `global.set()`, `global.get()`

## Tips for Learning

1. **Start Small**: Begin with simple flows and gradually add complexity
2. **Use Debug Nodes**: Add debug nodes to see message content
3. **Experiment**: Try different node configurations
4. **Read Documentation**: Check node descriptions and help text
5. **Practice**: Build your own projects based on the examples

## Common Patterns

### Data Processing
```javascript
// Get input data
var input = msg.payload;

// Process the data
var result = {
    original: input,
    processed: input.toUpperCase(),
    timestamp: new Date().toISOString()
};

// Update message
msg.payload = result;
return msg;
```

### State Management
```javascript
// Get current state
var state = context.get('myState') || {};

// Update state
state.value = msg.payload;
state.lastUpdate = new Date().toISOString();

// Store state
context.set('myState', state);

// Return updated message
msg.payload = state;
return msg;
```

### Error Handling
```javascript
try {
    var data = JSON.parse(msg.payload);
    msg.payload = data;
} catch (error) {
    node.error("JSON parse error: " + error.message);
    msg.payload = { error: "Invalid JSON" };
}
return msg;
```

## Next Steps

1. **Explore the Examples**: Import and study each flow file
2. **Modify Examples**: Change parameters and see what happens
3. **Build Your Own**: Create flows for your specific needs
4. **Join the Community**: Visit [Node-RED Forum](https://discourse.nodered.org/)
5. **Read More**: Check the [Node-RED Documentation](https://nodered.org/docs/)

Happy learning! 🚀