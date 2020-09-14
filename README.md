meta-adcsamplebeat
----

This is a recipe meta layer for Yocto that builds a custom elastic beat.
This beat is used in STM32MP157C. The CM4 firmware samples 4x ADC channels
using DMA and then sends the values using the OpenAMP IPC. The default
IPC implementation creates the `/dav/ttyRPMSG0` serial port that the Linux
user space can use to exchange data with the Cortex-M4 firmware.

Therefore, this beat opens a serial port and start receiving the continuous
stream from the CM4 MCU and then published the data to the configured
Elastisearch server.

You can read more info about this example [here](https://www.stupid-projects.com/using-elastic-stack-elk-on-embedded-part-2/).

## Supported versions
The current supported beat version is 
* v8.0.0

And it's tested with the v7.9.1 Elasticserver and Kibana

## Adding the meta-elastic-beats layer to your build

To add the layer to your build :

```sh
bitbake-layers add-layer meta-elastic-beats
```

Or just simply add the layer manually to your `bblayers.conf` file.

## Adding beats to your image
To add a beat in your image then add one of the following recipes to your
`IMAGE_INSTALL`:

```sh
IMAGE_INSTALL += "adcsamplebeat"
```

The configuration yaml files are the default ones. You need to override them
with a custom recipe and use your own for your specific usage. The configuration
Yaml files is located in `meta-adcsamplebeat/recipes-elastic-beats/adcsamplerbeat/adcsamplerbeat`.

## Known issues
The golang build changes the files in pkg/mod in to read only. This means that
bitbake is not able to delete those files if the build fails and you need to
delete the folder manually. Normally this is handled with the `go clean -modcache` comamnd. 

## Maintainer
Dimitris Tassopoulos <dimtass@gmail.com>