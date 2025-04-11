Consolidated MVP Endpoints
Sensor Management

POST /api/sensors - Create a new sensor
GET /api/sensors - List all sensors (with optional filters)
GET /api/sensors/{id} - Get sensor details by ID
PUT /api/sensors/{id} - Update sensor details
DELETE /api/sensors/{id} - Delete a sensor
GET /api/sensors/lookup/{name} - Get sensor ID by name

Sensor Groups

POST /api/groups - Create a sensor group
GET /api/groups - List all sensor groups
GET /api/groups/{id} - Get group details including member sensors
PUT /api/groups/{id} - Update group details
DELETE /api/groups/{id} - Delete a group
POST /api/groups/{id}/sensors/{sensorId} - Add sensor to group
DELETE /api/groups/{id}/sensors/{sensorId} - Remove sensor from group
GET /api/groups/lookup/{name} - Get group ID by name

Sensor Readings

GET /api/readings/{sensorId} - Get current reading for a sensor
GET /api/readings - Get current readings for all sensors
GET /api/readings/{sensorId}/history - Get historical readings with time range
GET /api/readings/group/{groupId} - Get current readings for all sensors in a group
GET /api/readings/aggregate - Get aggregated data by time interval

Logging Configuration

GET /api/logging/{sensorId} - Get logging status for a sensor
PUT /api/logging/{sensorId} - Configure logging for a sensor (frequency, etc.)
POST /api/logging/{sensorId}/start - Start logging for a sensor
POST /api/logging/{sensorId}/stop - Stop logging for a sensor
PUT /api/logging/group/{groupId} - Configure logging for a group
POST /api/logging/group/{groupId}/start - Start logging for all sensors in a group
POST /api/logging/group/{groupId}/stop - Stop logging for all sensors in a group

Dashboard Data

GET /api/dashboard/summary - Get overview statistics (sensor counts, active sessions, etc.)
GET /api/dashboard/active - Get list of actively logging sensors

This consolidated set of endpoints provides all the necessary functionality for the MVP, supporting both the core data management and the visualization needs of the dashboard UI.
