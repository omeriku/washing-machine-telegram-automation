# Washing Machine / Dryer Telegram Automation

![Screenshot 2021-04-19 155949](https://user-images.githubusercontent.com/73793617/115246199-574a8080-a12e-11eb-93be-3fc0e74a3d8e.png)


```yaml
## configuration.yaml ##

sensors:
  dryer_power:
    friendly_name_template: >-
      {% if states('sensor.shelly1pm_dryer_power')|float > 20 %}
        Dryer is Working Now
      {% else %}
        Dryer is ready to use
      {% endif %}
    icon_template: >-
      {% if states('sensor.shelly1pm_dryer_power')|float > 20 %}
        mdi:tumble-dryer
      {% else %}
        mdi:tumble-dryer-off
      {% endif %}
    value_template: >-
      {% if states('switch.shelly1pm_dryer')  != 'unavailable' %}
        {{ states('sensor.shelly1pm_dryer_power') }}
      {% else %}
        {{ -1 }}
      {% endif %}
    unit_of_measurement: "W"
 ```

<img src="https://user-images.githubusercontent.com/73793617/116567150-0a299400-a910-11eb-8b0a-f1d70392f338.jpg" width="50%">


```yaml
## automation.yaml ##

alias: Empty Washing machine
description: 'when someone press inline'
trigger:
  - platform: event
    event_data:
      command: /command
    event_type: telegram_callback
condition: []
action:
  - service: notify.telegram_home_bot
    data:
      message: "{{trigger.event.data.from_first}} {{ ("use", "your", "own", "random", "ideas") | random }}"
  - service: automation.turn_off
    target:
      entity_id:
        - automation.please_go_to_the_washing_machine
        - automation.empty_washing_machine
mode: single
```
