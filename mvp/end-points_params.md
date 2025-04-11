# Sensor Metadata for Modbus Communication

Given that the sensors use Modbus for communication, we need to include specific Modbus-related metadata in our sensor definitions. Here's a comprehensive metadata structure for the MVP:

## Sensor Metadata

```json
{
  "id": "integer",
  "name": "string",
  "description": "string",
  "group_id": "integer or null",
  "location": "string",
  "units": "string",
  "modbus_config": {
    "connection_type": "string (RTU or TCP)",
    "device_address": "integer (1-247)",
    "register_type": "string (holding, input, coil, discrete_input)",
    "register_address": "integer",
    "register_count": "integer",
    "data_type": "string (int16, uint16, int32, uint32, float32, etc.)",
    "byte_order": "string (big_endian or little_endian)",
    "ip_address": "string (for TCP connections)",
    "port": "integer (for TCP connections)",
    "serial_port": "string (for RTU connections)",
    "baud_rate": "integer (for RTU connections)",
    "parity": "string (for RTU connections)",
    "stop_bits": "integer (for RTU connections)",
    "timeout_ms": "integer"
  },
  "scaling": {
    "scale_factor": "float",
    "offset": "float"
  },
  "logging_config": {
    "enabled": "boolean",
    "interval_seconds": "integer",
    "retention_days": "integer"
  },
  "thresholds": {
    "low_critical": "float or null",
    "low_warning": "float or null",
    "high_warning": "float or null",
    "high_critical": "float or null"
  },
  "display_config": {
    "color": "string (hex color code)",
    "display_precision": "integer",
    "display_category": "string",
    "dashboard_order": "integer"
  },
  "created_at": "datetime",
  "updated_at": "datetime"
}
```

## API Endpoints with Parameters and Responses

### Sensor Management

#### Create Sensor
- **POST `/api/sensors`**
  - Input: Sensor metadata (JSON)
  - Output:
    ```json
    {
      "id": "integer",
      "name": "string",
      "success": true
    }
    ```

#### Get All Sensors
- **GET `/api/sensors`**
  - Query Parameters:
    - `group_id`: Filter by group ID
    - `location`: Filter by location
    - `connection_type`: Filter by Modbus connection type
  - Output:
    ```json
    {
      "sensors": [
        // Array of sensor metadata objects
      ],
      "count": "integer"
    }
    ```

#### Get Sensor by ID
- **GET `/api/sensors/{id}`**
  - Output: Full sensor metadata object

#### Update Sensor
- **PUT `/api/sensors/{id}`**
  - Input: Partial sensor metadata (fields to update)
  - Output:
    ```json
    {
      "id": "integer",
      "success": true
    }
    ```

#### Delete Sensor
- **DELETE `/api/sensors/{id}`**
  - Output:
    ```json
    {
      "success": true
    }
    ```

### Sensor Readings

#### Get Current Reading
- **GET `/api/readings/{sensorId}`**
  - Output:
    ```json
    {
      "sensor_id": "integer",
      "value": "float",
      "raw_value": "integer",
      "timestamp": "datetime",
      "status": "string (ok, warning, critical)",
      "units": "string"
    }
    ```

#### Get Historical Readings
- **GET `/api/readings/{sensorId}/history`**
  - Query Parameters:
    - `start_time`: ISO datetime
    - `end_time`: ISO datetime
    - `interval`: Aggregation interval in seconds (optional)
    - `limit`: Maximum number of readings to return
  - Output:
    ```json
    {
      "sensor_id": "integer",
      "readings": [
        {
          "timestamp": "datetime",
          "value": "float",
          "status": "string"
        }
      ]
    }
    ```

### Logging Configuration

#### Set Logging Configuration
- **PUT `/api/logging/{sensorId}`**
  - Input:
    ```json
    {
      "enabled": "boolean",
      "interval_seconds": "integer",
      "retention_days": "integer"
    }
    ```
  - Output:
    ```json
    {
      "success": true,
      "sensor_id": "integer"
    }
    ```

#### Start Logging
- **POST `/api/logging/{sensorId}/start`**
  - Output:
    ```json
    {
      "success": true,
      "sensor_id": "integer",
      "logging_started_at": "datetime"
    }
    ```

#### Stop Logging
- **POST `/api/logging/{sensorId}/stop`**
  - Output:
    ```json
    {
      "success": true,
      "sensor_id": "integer",
      "logging_stopped_at": "datetime",
      "total_readings_logged": "integer"
    }
    ```

### Group Operations

#### Create Group
- **POST `/api/groups`**
  - Input:
    ```json
    {
      "name": "string",
      "description": "string"
    }
    ```
  - Output:
    ```json
    {
      "id": "integer",
      "name": "string",
      "success": true
    }
    ```

#### Get Group Members
- **GET `/api/groups/{id}`**
  - Output:
    ```json
    {
      "id": "integer",
      "name": "string",
      "description": "string",
      "sensors": [
        // Array of sensor IDs and names
      ],
      "sensor_count": "integer"
    }
    ```

This metadata structure and API parameters accommodate the Modbus communication requirements while providing all necessary data for the sensor monitoring dashboard MVP.
