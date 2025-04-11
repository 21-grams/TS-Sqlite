
Core Endpoints:

Create a sensor:

POST /sensors - Create a new analog sensor with metadata


Read sensors:

GET /sensors - Get all sensors (with optional filters)
GET /sensors/{id} - Get a specific sensor by ID
GET /sensors/readings - Get current readings for all sensors
GET /sensors/{id}/readings - Get current reading for a specific sensor


Config sensor logging:

PUT /sensors/{id}/logging/config - Set logging parameters (frequency, etc.)
PUT /sensors/group/{groupId}/logging/config - Configure logging for a group


Enable logging:

POST /sensors/{id}/logging/start - Start logging for a sensor
POST /sensors/group/{groupId}/logging/start - Start logging for a group


Disable logging:

POST /sensors/{id}/logging/stop - Stop logging for a sensor
POST /sensors/group/{groupId}/logging/stop - Stop logging for a group


Update a sensor:

PUT /sensors/{id} - Update sensor details


Delete a sensor:

DELETE /sensors/{id} - Delete a sensor
DELETE /sensors/group/{groupId} - Delete sensors by group ID


Get ID by name:

GET /sensors/lookup/{name} - Get sensor ID by name


Get group ID by name:

GET /sensor-groups/lookup/{name} - Get group ID by name



Additional Endpoints I Recommend:

Sensor Groups Management:

POST /sensor-groups - Create a sensor group
GET /sensor-groups - List all sensor groups
PUT /sensor-groups/{id} - Update a sensor group
POST /sensor-groups/{id}/sensors/{sensorId} - Add sensor to group


Sensor Readings History:

GET /sensors/{id}/readings/history - Get historical readings with time range


Logging Status:

GET /sensors/{id}/logging/status - Check if logging is active



Are there any specific additional operations you'd like to include or modify in this API design?
