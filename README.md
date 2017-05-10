# If This Then That (IFTTT) Concourse Resource

Pushes Maker events to the [IFTTT maker webhook](https://ifttt.com/maker_webhooks) trigger. 

***NOTE:** `put` is the only action that is supported for now.* Future iterations may include triggering this resource
 from IFTTT. Pull requests are welcome.


## Source configuration

- `ifttt_key` required

**Example `resource_type` and `source` configuration**:

```.yml
resource_types:
- name: ifttt-integration
  type: docker-image
  source:
    repository: switzerswish/ifttt-resource
    tag: latest

resources:
- name: ifttt
  type: ifttt-integration
  source:
    ifttt_key: {{ifttt_api_key}}
```

The double curly brackets are a result of injecting `ifttt_api_key` from a separate secrets.yml file via the
`--load-vars-from=` key of the `fly set-pipeline` command.


## `put` configuration

- `event_name` required

**Example usage in a `put` step**

```.yml

jobs:
- name: example-job
  plan:
  - task: say-hello
    file: ...
    on_success:
      put: ifttt
      params:
        event_name: "<YOUR PASSING BUILD EVENT NAME HERE>"
    on_failure:
      put: ifttt
      params:
        event_name: "<YOUR FAILING BUILD EVENT NAME HERE>"
```

Clone this example and run it for yourself [here](https://github.com/oliverswitzer/ifttt-concourse-resource-example).