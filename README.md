![stability-wip](https://img.shields.io/badge/stability-work_in_progress-lightgrey.svg?style=for-the-badge)


[![Version](https://img.shields.io/badge/version-0.0.6-green.svg?style=for-the-badge)](#) [![maintained](https://img.shields.io/maintenance/yes/2019.svg?style=for-the-badge)](#)

[![maintainer](https://img.shields.io/badge/maintainer-Isabella%20Alström%20%40isabellaalstrom-blue.svg?style=for-the-badge)](#)

## Version 0.0.6 contains changes according to the new component scheme released with v.0.88 of Home Assistant.

# sensor.krisinformation
Component to get Krisinformation for [Home Assistant](https://www.home-assistant.io/).

Will get all messages from [Krisinformations api](http://api.krisinformation.se/v2/feed?format=json) in a set radius from your coordinates.
If one of the fetched messages is an alert as opposed to news, the state of the sensor will be "Alert". The sensor contains all fetched messages as objects.

Use together with [custom card for Lovelace](https://github.com/isabellaalstrom/krisinfo-card).

<img src="https://github.com/isabellaalstrom/krisinfo-card/blob/master/krisinfo.png" alt="Krisinformation Lovelace Card" />

This component is supported by [Custom updater and Tracker card](https://github.com/custom-components/custom_updater).

## Installation:

1. Install this component by creating a `custom_components` folder in the same folder as your configuration.yaml is, if you don't already have one.
2. Inside that folder, create another folder named `krisinformation`. Put the `sensor.py` file in there (if you copy and paste the code, make sure you do it from the [raw version](https://raw.githubusercontent.com/isabellaalstrom/sensor.krisinformation/master/custom_components/krisinformation/sensor.py) of the file).
2. Add the code to your `configuration.yaml` using the config options below.
3. **You will need to restart after installation for the component to start working.**

* If you're having issues, [ask for help on the forums](https://community.home-assistant.io/t/custom-component-krisinformation-sweden/90340) or post an issue.

**Configuration variables:**

key | type | description
:--- | :--- | :---
**platform (Required)** | string | `krisinformation`
**latitude (Required)** | string | The latitude of the position from which the sensor should look for messages.
**longitude (Required)** | string | The longitude of the position from which the sensor should look for messages.
**name (Optional)** | string | Custom name for the sensor. Default `krisinformation`
**radius (Optional)** | number | The radius in km from your position that the sensor should look for messages. Default `50`


**Example configuration.yaml:**

```yaml
sensor:
  - platform: krisinformation
    name: Krisinformation Stockholm
    latitude: !secret lat_coord
    
    longitude: !secret long_coord
    radius: 100
```

***

## Usage

**Example automation for getting a notification when the sensor has an alert:**
Make sure you change the sensor name if you didn't use the default name.

```yaml
automation:
  - alias: 'Krisinformation Alert'
    initial_state: 'on'
    trigger:
      platform: state
      entity_id: sensor.krisinformation
      to: "Alert"
    action:
      - service: notify.my_phone
        data_template:
          message: >
            {{states.sensor.krisinformation.attributes.messages[0].Headline}} - {{states.sensor.krisinformation.attributes.messages[0].Message}} {{states.sensor.krisinformation.attributes.messages[0].Web}}
```

Like my work and want to say thanks? Do it here:

<a href="https://www.buymeacoffee.com/iq1f96D" target="_blank"><img src="https://www.buymeacoffee.com/assets/img/custom_images/purple_img.png" alt="Buy Me A Coffee" style="height: auto !important;width: auto !important;" ></a>
