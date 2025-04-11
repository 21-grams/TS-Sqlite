Key Components of the API Specification:
1. Core CRUD Operations

Sensor Management: Create, read, update, and delete sensor metadata
Data Ingestion: Log individual readings or bulk import large datasets
Data Retrieval: Query readings with comprehensive filtering options
Data Deletion: Manage data retention with time-based deletion

2. Visualization-Specific Endpoints

Time-Series Data: Optimized for line charts with optional moving averages
Box-and-Whisker Data: Statistical distributions with quartiles for temperature analysis
Sankey Diagram Data: Energy/resource flow visualization between sources and destinations
Pareto Chart Data: Consumption analysis with cumulative percentages

3. Analytics Endpoints

Pattern Detection: Identify usage patterns, cycles, and correlations between sensors
Anomaly Detection: Find outliers and unexpected readings with configurable sensitivity
Statistical Reports: Generate comprehensive summaries grouped by time periods

4. Dashboard/Mobile Support

Current Status: Get the latest reading from each sensor
Dashboard Summary: Compact overview of all system metrics
Live Readings: Real-time updates using Server-Sent Events

5. Implementation Details

Transaction handling for optimal database performance
Caching strategies for frequently accessed data
Client-side integration examples for charts and real-time updates



# API Specification for Sensor Monitoring System

This specification defines the CRUD operations and visualization endpoints for the industrial sensor monitoring database. The API provides a structured interface for data ingestion, retrieval, and management of the time-series and event-based sensor data.

## 1. Core CRUD Operations

### Sensor Management

#### Create Sensor

- **Endpoint:** `POST /api/sensors`
- **Description:** Register a new sensor in the system
- **Request Body:**
```json
{
  "sensor_id": 1,
  "sensor_name": "Temperature Sensor 1",
  "sensor_type": "temperature",
  "location": "Building A, Room 101",
  "unit": "C",
  "threshold_min": 18.0,
  "threshold_max": 25.0,
  "calibration_date": 1712735400,
  "notes": "Installed April 2025"
}
```
- **Response:**
```json
{
  "success": true,
  "sensor_id": 1
}
```

#### Get All Sensors

- **Endpoint:** `GET /api/sensors`
- **Description:** Retrieve information about all registered sensors
- **Query Parameters:**
  - `type` (optional): Filter by sensor type
  - `location` (optional): Filter by location
- **Response:**
```json
{
  "sensors": [
    {
      "sensor_id": 1,
      "sensor_name": "Temperature Sensor 1",
      "sensor_type": "temperature",
      "location": "Building A, Room 101",
      "unit": "C",
      "threshold_min": 18.0,
      "threshold_max": 25.0,
      "calibration_date": 1712735400,
      "notes": "Installed April 2025"
    },
    {
      "sensor_id": 2,
      "sensor_name": "Flow Meter 1",
      "sensor_type": "flow",
      "location": "Building A, Utility Room",
      "unit": "L/min",
      "threshold_min": 5.0,
      "threshold_max": 50.0,
      "calibration_date": 1712649000,
      "notes": "High precision model"
    }
  ]
}
```

#### Get Sensor by ID

- **Endpoint:** `GET /api/sensors/:id`
- **Description:** Retrieve information about a specific sensor
- **Response:**
```json
{
  "sensor_id": 1,
  "sensor_name": "Temperature Sensor 1",
  "sensor_type": "temperature",
  "location": "Building A, Room 101",
  "unit": "C",
  "threshold_min": 18.0,
  "threshold_max": 25.0,
  "calibration_date": 1712735400,
  "notes": "Installed April 2025"
}
```

#### Update Sensor

- **Endpoint:** `PUT /api/sensors/:id`
- **Description:** Update sensor metadata
- **Request Body:**
```json
{
  "sensor_name": "Temperature Sensor 1 - Primary",
  "threshold_min": 17.5,
  "threshold_max": 26.0,
  "notes": "Recalibrated April 11, 2025"
}
```
- **Response:**
```json
{
  "success": true,
  "sensor_id": 1
}
```

#### Delete Sensor

- **Endpoint:** `DELETE /api/sensors/:id`
- **Description:** Remove a sensor from the system
- **Response:**
```json
{
  "success": true,
  "sensor_id": 1
}
```

### Sensor Readings

#### Log Sensor Reading(s)

- **Endpoint:** `POST /api/readings`
- **Description:** Record one or multiple sensor readings
- **Request Body:**
```json
{
  "readings": [
    {
      "timestamp": 1712921800,
      "sensor_id": 1,
      "value": 21.5,
      "change_type": "periodic"
    },
    {
      "timestamp": 1712921800,
      "sensor_id": 9,
      "state": 1,
      "change_type": "event"
    }
  ]
}
```
- **Response:**
```json
{
  "success": true,
  "inserted_count": 2
}
```

#### Bulk Import Readings

- **Endpoint:** `POST /api/readings/bulk`
- **Description:** Import a large batch of sensor readings (optimized for performance)
- **Request Body:** Binary format or compressed JSON with array of readings
- **Response:**
```json
{
  "success": true,
  "imported_count": 5000,
  "errors_count": 0
}
```

#### Get Readings

- **Endpoint:** `GET /api/readings`
- **Description:** Retrieve sensor readings based on filters
- **Query Parameters:**
  - `sensor_id` (optional): Specific sensor ID
  - `sensor_type` (optional): Type of sensor
  - `start_time` (required): Start of time range (Unix timestamp)
  - `end_time` (required): End of time range (Unix timestamp)
  - `limit` (optional): Maximum number of readings to return (default: 1000)
  - `offset` (optional): Pagination offset
  - `aggregation` (optional): Time-based aggregation ('hour', 'day', 'month')
- **Response:**
```json
{
  "readings": [
    {
      "id": 12345,
      "timestamp": 1712921800,
      "sensor_id": 1,
      "sensor_name": "Temperature Sensor 1",
      "sensor_type": "temperature",
      "value": 21.5,
      "unit": "C",
      "change_type": "periodic"
    },
    {
      "id": 12346,
      "timestamp": 1712922400,
      "sensor_id": 1,
      "sensor_name": "Temperature Sensor 1",
      "sensor_type": "temperature",
      "value": 21.7,
      "unit": "C",
      "change_type": "periodic"
    }
  ],
  "total_count": 24,
  "page": 1,
  "limit": 1000
}
```

#### Delete Readings

- **Endpoint:** `DELETE /api/readings`
- **Description:** Remove sensor readings in a time range (admin only)
- **Query Parameters:**
  - `sensor_id` (optional): Specific sensor ID
  - `start_time` (required): Start of time range to delete
  - `end_time` (required): End of time range to delete
- **Response:**
```json
{
  "success": true,
  "deleted_count": 120
}
```

## 2. Visualization and Analytics Endpoints

### Current Status

#### Get Current Status

- **Endpoint:** `GET /api/status/current`
- **Description:** Get the latest reading from each sensor
- **Response:**
```json
{
  "timestamp": 1712921800,
  "sensors": [
    {
      "sensor_id": 1,
      "sensor_name": "Temperature Sensor 1",
      "sensor_type": "temperature",
      "value": 21.5,
      "unit": "C",
      "updated_at": 1712921800,
      "out_of_threshold": false
    },
    {
      "sensor_id": 9,
      "sensor_name": "Light Status 1",
      "sensor_type": "light",
      "state": 1,
      "updated_at": 1712921800,
      "out_of_threshold": false
    }
  ]
}
```

#### Get Health Status

- **Endpoint:** `GET /api/status/health`
- **Description:** Get the health status of all sensors
- **Response:**
```json
{
  "timestamp": 1712921800,
  "healthy_count": 30,
  "warning_count": 2,
  "critical_count": 0,
  "sensors_warning": [
    {
      "sensor_id": 3,
      "sensor_name": "Temperature Sensor 3",
      "current_value": 26.5,
      "threshold_max": 25.0,
      "status": "warning"
    },
    {
      "sensor_id": 17,
      "sensor_name": "Flow Meter 1",
      "current_value": 4.8,
      "threshold_min": 5.0,
      "status": "warning"
    }
  ],
  "sensors_critical": []
}
```

### Time-Series Visualization

#### Get Time-Series Data

- **Endpoint:** `GET /api/visualizations/time-series`
- **Description:** Retrieve time-series data optimized for line charts
- **Query Parameters:**
  - `sensor_ids` (required): Comma-separated list of sensor IDs
  - `start_time` (required): Start of time range
  - `end_time` (required): End of time range
  - `interval` (optional): Data interval ('minute', 'hour', 'day', 'week', 'month')
  - `include_moving_average` (optional): Include 12-point moving average
- **Response:**
```json
{
  "labels": ["2025-04-01 08:00", "2025-04-01 09:00", "2025-04-01 10:00"],
  "datasets": [
    {
      "sensor_id": 1,
      "sensor_name": "Temperature Sensor 1",
      "unit": "C",
      "data": [21.0, 21.5, 22.0],
      "moving_average": [21.2, 21.4, 21.7]
    },
    {
      "sensor_id": 2,
      "sensor_name": "Temperature Sensor 2",
      "unit": "C",
      "data": [22.5, 23.0, 23.5],
      "moving_average": [22.7, 22.9, 23.2]
    }
  ]
}
```

#### Get Box-and-Whisker Data

- **Endpoint:** `GET /api/visualizations/box-whisker`
- **Description:** Retrieve statistical distribution data for box-and-whisker plots
- **Query Parameters:**
  - `sensor_ids` (required): Comma-separated list of sensor IDs
  - `start_time` (required): Start of time range
  - `end_time` (required): End of time range
  - `group_by` (optional): Grouping interval ('day', 'week', 'month')
- **Response:**
```json
{
  "datasets": [
    {
      "sensor_id": 1,
      "sensor_name": "Temperature Sensor 1",
      "unit": "C",
      "data": [
        {
          "group": "2025-04-01",
          "min": 19.5,
          "q1": 20.7,
          "median": 21.5,
          "q3": 22.3,
          "max": 23.1,
          "outliers": [18.2, 24.7]
        },
        {
          "group": "2025-04-02",
          "min": 19.8,
          "q1": 21.0,
          "median": 21.8,
          "q3": 22.5,
          "max": 23.4,
          "outliers": []
        }
      ]
    }
  ]
}
```

#### Get Sankey Diagram Data

- **Endpoint:** `GET /api/visualizations/sankey`
- **Description:** Retrieve flow data for Sankey diagrams
- **Query Parameters:**
  - `sensor_type` (required): Type of sensor ('power', 'flow')
  - `start_time` (required): Start of time range
  - `end_time` (required): End of time range
  - `metric` (optional): 'consumption', 'time_of_day', 'location'
- **Response:**
```json
{
  "nodes": [
    {"id": "sensor_1", "name": "Power Sensor 1"},
    {"id": "sensor_2", "name": "Power Sensor 2"},
    {"id": "morning", "name": "Morning (6-12)"},
    {"id": "afternoon", "name": "Afternoon (12-18)"},
    {"id": "evening", "name": "Evening (18-24)"},
    {"id": "night", "name": "Night (0-6)"}
  ],
  "links": [
    {"source": "sensor_1", "target": "morning", "value": 25.7},
    {"source": "sensor_1", "target": "afternoon", "value": 42.3},
    {"source": "sensor_1", "target": "evening", "value": 37.1},
    {"source": "sensor_1", "target": "night", "value": 11.4},
    {"source": "sensor_2", "target": "morning", "value": 18.2},
    {"source": "sensor_2", "target": "afternoon", "value": 31.5},
    {"source": "sensor_2", "target": "evening", "value": 28.4},
    {"source": "sensor_2", "target": "night", "value": 9.8}
  ]
}
```

#### Get Pareto Chart Data

- **Endpoint:** `GET /api/visualizations/pareto`
- **Description:** Retrieve data for Pareto analysis
- **Query Parameters:**
  - `sensor_type` (required): Type of sensor to analyze
  - `start_time` (required): Start of time range
  - `end_time` (required): End of time range
  - `metric` (required): Metric to analyze ('consumption', 'events', 'anomalies')
- **Response:**
```json
{
  "items": [
    {
      "name": "Power Sensor 3",
      "value": 457.2,
      "percentage": 35.8,
      "cumulative_percentage": 35.8
    },
    {
      "name": "Power Sensor 1",
      "value": 312.5,
      "percentage": 24.5,
      "cumulative_percentage": 60.3
    },
    {
      "name": "Power Sensor 7",
      "value": 245.8,
      "percentage": 19.2,
      "cumulative_percentage": 79.5
    },
    {
      "name": "Power Sensor 2",
      "value": 141.6,
      "percentage": 11.1,
      "cumulative_percentage": 90.6
    },
    {
      "name": "Power Sensor 5",
      "value": 120.3,
      "percentage": 9.4,
      "cumulative_percentage": 100.0
    }
  ],
  "total": 1277.4,
  "unit": "kWh"
}
```

### Pattern Detection

#### Get Usage Patterns

- **Endpoint:** `GET /api/analytics/patterns/usage`
- **Description:** Detect usage patterns and cycles in sensor data
- **Query Parameters:**
  - `sensor_ids` (required): Comma-separated list of sensor IDs
  - `start_time` (required): Start of time range
  - `end_time` (required): End of time range
  - `pattern_type` (optional): 'daily', 'weekly', 'correlated'
- **Response:**
```json
{
  "sensor_id": 1,
  "sensor_name": "Temperature Sensor 1",
  "patterns": [
    {
      "type": "daily_cycle",
      "confidence": 0.87,
      "peak_time": "14:30",
      "trough_time": "04:15",
      "average_amplitude": 4.2,
      "unit": "C"
    },
    {
      "type": "weekly_pattern",
      "confidence": 0.76,
      "details": {
        "weekday_avg": 22.5,
        "weekend_avg": 20.8,
        "highest_day": "Wednesday",
        "lowest_day": "Sunday"
      }
    }
  ],
  "correlations": [
    {
      "sensor_id": 9,
      "sensor_name": "Light Status 1",
      "correlation": 0.72,
      "lag_minutes": 15,
      "description": "Temperature rises ~15 minutes after lights turn on"
    }
  ]
}
```

#### Get Anomaly Detection

- **Endpoint:** `GET /api/analytics/anomalies`
- **Description:** Detect anomalies in sensor readings
- **Query Parameters:**
  - `sensor_ids` (required): Comma-separated list of sensor IDs
  - `start_time` (required): Start of time range
  - `end_time` (required): End of time range
  - `sensitivity` (optional): Anomaly detection sensitivity (1-10, default: 5)
- **Response:**
```json
{
  "anomalies": [
    {
      "sensor_id": 1,
      "sensor_name": "Temperature Sensor 1",
      "timestamp": 1712835400,
      "value": 28.5,
      "expected_range": [20.5, 25.5],
      "deviation_percentage": 11.8,
      "severity": "high"
    },
    {
      "sensor_id": 17,
      "sensor_name": "Flow Meter 1",
      "timestamp": 1712772600,
      "value": 2.1,
      "expected_range": [5.0, 50.0],
      "deviation_percentage": 58.0,
      "severity": "critical"
    }
  ],
  "total_readings_analyzed": 2560,
  "anomaly_percentage": 0.078
}
```

### Historical Reports

#### Get Statistics Report

- **Endpoint:** `GET /api/reports/statistics`
- **Description:** Generate statistical summary report
- **Query Parameters:**
  - `sensor_ids` (optional): Comma-separated list of sensor IDs
  - `sensor_type` (optional): Type of sensor
  - `start_time` (required): Start of time range
  - `end_time` (required): End of time range
  - `group_by` (optional): Grouping interval ('day', 'week', 'month')
- **Response:**
```json
{
  "period": {
    "start_time": 1712188800,
    "end_time": 1712966400,
    "description": "Apr 2025, Week 1-2"
  },
  "statistics": [
    {
      "sensor_id": 1,
      "sensor_name": "Temperature Sensor 1",
      "sensor_type": "temperature",
      "unit": "C",
      "min": 18.5,
      "max": 25.7,
      "avg": 22.3,
      "std_dev": 1.4,
      "readings_count": 672,
      "groups": [
        {
          "group": "2025-04-01",
          "min": 19.1,
          "max": 24.6,
          "avg": 21.9
        },
        {
          "group": "2025-04-02",
          "min": 18.5,
          "max": 25.2,
          "avg": 22.1
        }
      ]
    }
  ]
}
```

#### Get Consumption Report

- **Endpoint:** `GET /api/reports/consumption`
- **Description:** Generate consumption summary for power or flow sensors
- **Query Parameters:**
  - `sensor_ids` (optional): Comma-separated list of sensor IDs
  - `sensor_type` (required): 'power' or 'flow'
  - `start_time` (required): Start of time range
  - `end_time` (required): End of time range
  - `group_by` (optional): Grouping interval ('day', 'week', 'month')
- **Response:**
```json
{
  "period": {
    "start_time": 1712188800,
    "end_time": 1712966400,
    "description": "Apr 2025, Week 1-2"
  },
  "total_consumption": 1452.7,
  "unit": "kWh",
  "sensors": [
    {
      "sensor_id": 25,
      "sensor_name": "Power Sensor 1",
      "consumption": 457.2,
      "percentage": 31.5,
      "groups": [
        {
          "group": "2025-04-01",
          "consumption": 72.5
        },
        {
          "group": "2025-04-02",
          "consumption": 68.3
        }
      ]
    },
    {
      "sensor_id": 26,
      "sensor_name": "Power Sensor 2",
      "consumption": 312.5,
      "percentage": 21.5,
      "groups": [
        {
          "group": "2025-04-01",
          "consumption": 51.2
        },
        {
          "group": "2025-04-02",
          "consumption": 48.9
        }
      ]
    }
  ],
  "comparison": {
    "previous_period": 1378.4,
    "change_percentage": 5.4,
    "trend": "increasing"
  }
}
```

## 3. System Management Endpoints

#### Get Database Health

- **Endpoint:** `GET /api/system/health`
- **Description:** Get the health status of the database (admin only)
- **Response:**
```json
{
  "status": "healthy",
  "database_size_mb": 1245.7,
  "free_space_mb": 10547.2,
  "last_backup": 1712880000,
  "readings_count": 3125478,
  "oldest_reading": 1680307200,
  "newest_reading": 1712923200,
  "average_insert_rate": 35.2,
  "peak_insert_rate": 127.5
}
```

#### Run Maintenance

- **Endpoint:** `POST /api/system/maintenance`
- **Description:** Run database maintenance tasks (admin only)
- **Request Body:**
```json
{
  "tasks": ["analyze", "optimize", "vacuum"],
  "archive_before": 1680307200
}
```
- **Response:**
```json
{
  "success": true,
  "tasks_completed": ["analyze", "optimize", "vacuum"],
  "archived_readings": 1250365,
  "duration_seconds": 45.2,
  "new_database_size_mb": 1125.4
}
```

#### Export Data

- **Endpoint:** `GET /api/system/export`
- **Description:** Export sensor data to various formats
- **Query Parameters:**
  - `sensor_ids` (optional): Comma-separated list of sensor IDs
  - `start_time` (required): Start of time range
  - `end_time` (required): End of time range
  - `format` (optional): 'json', 'csv', 'excel'
- **Response:** Binary file download

## 4. Mobile/Dashboard Specific Endpoints

#### Get Dashboard Summary

- **Endpoint:** `GET /api/dashboard/summary`
- **Description:** Get a summary of all essential data for dashboard display
- **Response:**
```json
{
  "timestamp": 1712921800,
  "sensor_counts": {
    "total": 32,
    "temperature": 8,
    "flow": 8,
    "power": 8,
    "light": 4,
    "fridge_door": 4
  },
  "status": {
    "normal": 29,
    "warning": 2,
    "critical": 1
  },
  "current_consumption": {
    "power_kw": 27.5,
    "flow_lpm": 124.2
  },
  "today_consumption": {
    "power_kwh": 452.7,
    "flow_l": 132560
  },
  "recent_events": [
    {
      "timestamp": 1712921200,
      "sensor_name": "Fridge Door 2",
      "event": "Door opened",
      "duration_seconds": null
    },
    {
      "timestamp": 1712919400,
      "sensor_name": "Fridge Door 1",
      "event": "Door closed",
      "duration_seconds": 75
    }
  ],
  "alerts": [
    {
      "timestamp": 1712918500,
      "sensor_name": "Power Sensor 8",
      "message": "Critical: Excessive power consumption detected",
      "value": 12.7,
      "threshold": 10.0,
      "severity": "critical"
    }
  ]
}
```

#### Get Live Readings

- **Endpoint:** `GET /api/readings/live`
- **Description:** Get real-time readings using Server-Sent Events (SSE)
- **Query Parameters:**
  - `sensor_ids` (optional): Comma-separated list of sensor IDs
- **Response:** SSE stream of JSON objects:
```json
{
  "timestamp": 1712923200,
  "readings": [
    {
      "sensor_id": 1,
      "value": 22.7
    },
    {
      "sensor_id": 9,
      "state": 1
    }
  ]
}
```

## 5. Implementation Considerations

### Database Transactions

For each API endpoint, the implementation should utilize SQLite transactions appropriately:

1. **Read operations** (GET endpoints):
   - Use `BEGIN TRANSACTION` in read-only mode
   - Apply proper indices based on query parameters
   - Use prepared statements with parameter binding

2. **Write operations** (POST, PUT, DELETE endpoints):
   - Use `BEGIN IMMEDIATE TRANSACTION` for write operations
   - Group multiple operations within a single transaction
   - Apply proper error handling and rollback on failure

### Connection Pooling

To optimize concurrent access:

1. Implement a connection pool with reasonable limits (e.g., 10-20 connections)
2. Apply connection timeouts and idle connection recycling
3. Use dedicated connections for long-running analytical queries

### Caching Strategy

Implement caching for frequently accessed data:

1. **High-frequency cache** (1-minute TTL):
   - Current status responses
   - Dashboard summary data

2. **Medium-frequency cache** (10-minute TTL):
   - Visualization data for the current day
   - Recent events list

3. **Low-frequency cache** (1-hour TTL):
   - Historical reports
   - Statistical aggregations

### Data Preprocessing

For visualization endpoints:

1. Apply appropriate time-zone handling
2. Pre-calculate derivative values (rates of change, running averages)
3. Filter out anomalous data points that could skew visualizations
4. Apply data thinning for large time ranges (e.g., pick representative points)

### Security Considerations

1. Apply authentication and authorization for all endpoints
2. Implement rate limiting to prevent abuse
3. Apply input validation to prevent SQL injection
4. Log all write operations for audit purposes

## 6. Client-Side Integration Examples

### Dashboard Time-Series Chart

```javascript
// Example client-side code to fetch and render a time-series chart
async function loadTemperatureChart() {
  const response = await fetch('/api/visualizations/time-series?sensor_ids=1,2,3&start_time=1712188800&end_time=1712966400&interval=hour&include_moving_average=true');
  const data = await response.json();
  
  const chart = new Chart(document.getElementById('temperatureChart'), {
    type: 'line',
    data: {
      labels: data.labels,
      datasets: data.datasets.map(dataset => ({
        label: dataset.sensor_name,
        data: dataset.data,
        borderColor: getColorForSensor(dataset.sensor_id),
        fill: false
      }))
    },
    options: {
      responsive: true,
      title: {
        display: true,
        text: 'Temperature Readings'
      },
      scales: {
        x: {
          display: true,
          title: {
            display: true,
            text: 'Time'
          }
        },
        y: {
          display: true,
          title: {
            display: true,
            text: 'Temperature (°C)'
          }
        }
      }
    }
  });
}
```

### Real-Time Updates with Server-Sent Events

```javascript
// Example client-side code to handle real-time updates
function subscribeToLiveReadings() {
  const eventSource = new EventSource('/api/readings/live?sensor_ids=1,2,3,9,10');
  
  eventSource.onmessage = function(event) {
    const data = JSON.parse(event.data);
    
    // Update UI with new readings
    data.readings.forEach(reading => {
      if ('value' in reading) {
        updateAnalogReading(reading.sensor_id, reading.value);
      } else if ('state' in reading) {
        updateDigitalStatus(reading.sensor_id, reading.state);
      }
    });
    
    // Update last update timestamp
    document.getElementById('lastUpdate').textContent = 
      new Date(data.timestamp * 1000).toLocaleTimeString();
  };
  
  eventSource.onerror = function() {
    console.error('SSE connection error');
    // Attempt to reconnect after delay
    setTimeout(() => subscribeToLiveReadings(), 5000);
    eventSource.close();
  };
}
```

This API specification provides a comprehensive interface for all required CRUD operations, visualization needs, and system management functions for the sensor monitoring database.
