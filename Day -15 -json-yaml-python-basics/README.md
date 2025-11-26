# python-json-yaml-fundamentals
---
#### Json (JavaScript Object Notation) 
 - JSON is a lightweight, text-based format for storing and exchanging data. 
 - It is easy for humans to read and write, and easy for machines to parse and generate.
 ```
{
  "name": "Dinesh",
  "age": 21,
  "skills": ["Python", "Mysql"]
}
```

 #### Yaml  (YAML Ain’t Markup Language)
 - YAML is a human friendly, text based format used for data storage and configuration. 
 - It is designed to be easy to read and write, using indentation instead of brackets or commas.
```
name: Dinesh
age: 25
skills:
  - Python
  - DevOps
```
---

##### Python JSON & YAML Libraries
 - Python provides built-in and external libraries to efficiently handle structured data in JSON and YAML formats. The main libraries used are,
 1. json (built-in for JSON)
 2. PyYAML (external for YAML)
---

#### 1. Load and Parse JSON Files from Telecom APIs
 - To load and parse JSON files obtained from Telecom APIs.
```
import yaml
# Open and load the YAML file
with open("docker-compose-basic-nrf.yaml") as f:
    data = yaml.safe_load(f)

# Display the loaded data (optional)
print(data)
```
 - The JSON file using Python’s json module and convert its contents into a Python dictionary for further processing.
---
#### 2.  Convert YAML → JSON and JSON → YAML
 - The code loads a YAML file, parses it, extracts an environment variable list, converts it to a dictionary, and saves the entire YAML structure as a formatted JSON file.
```
import yaml
import json

# Load YAML
with open("docker-compose-basic-nrf.yaml") as f:
    data = yaml.safe_load(f)

# Extract environment list
env_list = data["services"]["oai-amf"]["environment"]

# Convert environment list ("KEY=VALUE") → dictionary
env_dict = dict(item.split("=", 1) 
    for item in env_list)
print(env_dict["TZ"])

# -----------------------------
# Convert entire YAML → JSON
# -----------------------------
json_data = json.dumps(data, indent=4)

# Save to output.json file
with open("output.json", "w") as f:
    f.write(json_data)
```
 - Load and parse YAML, extract and process specific data.
 - Convert the complete YAML structure to JSON and write it to output.json.

#### 3. Extract Nested Fields from API Responses
 - To extract nested fields (like IP addresses) from a complex YAML structure, such as a Docker Compose file from telecom APIs.

```
import yaml

with open("docker-compose-basic-nrf.yaml") as f:
    data = yaml.safe_load(f)

services = data.get("services", {})

for name, svc in services.items():
    ip = (
        svc.get("networks", {})
           .get("public_net", {})
           .get("ipv4_address")
    )
    print(f"{name}  -->  {ip}")
```
 - For each service, attempts to retrieve the nested IPv4 address from networks → public_net → ipv4_address.
---

#### 4. Build a JSON-based configuration generator for router/network test setups

```
import yaml

with open("docker-compose-basic-nrf.yaml") as f:
    data = yaml.safe_load(f)

services = data.get("services", {})

for name, svc in services.items():
    volumes = svc.get("volumes", [])
    if volumes:
        print(f"{name}  -->  {', '.join(volumes)}")
    else:
        print(f"{name}  -->  No volumes")
```
 - This script reads a YAML file iterates through each service, and extracts the volumes section for each service. If no volumes are defined, it reports that as well.


---
