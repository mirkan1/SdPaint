# SdPaint

A Python script that lets you paint on a canvas and sends that image every stroke to the automatic1111 API and updates
the canvas when the image is generated.

## Controls

| Key / Mouse button            | Control                                            |
|-------------------------------|----------------------------------------------------|
| Left button                   | Draw with the current brush size                   |
| Middle button                 | Draw with a white color brush                      |
| `e` + Left button             | Eraser brush (bigger)                              |
| `z` + Left button             | Draw zone to be erased                             |
| `s` + Left button             | Draw zone to be kept                               |
| Scroll up / down              | Increase / decrease brush size                     |
| `1` to `9`                    | Set brush size                                     |
| `backspace`                   | Erase the entire sketch                            |
| `shift` + Left button         | Draw a line between two clicks                     |
| `RETURN` or `ENTER`           | Request image rendering                            |
| `ctrl` + `i`                  | Interrupt image rendering                          |
| `c`                           | Display current configuration while pressed        |
| `p`                           | Edit prompt                                        |
| `alt` + `p`                   | Edit negative prompt                               |
| `a`                           | Toggle autosave                                    |
| `shift` + `t`                 | Cycle render wait time (+0.5s, or off)             |
| `ctrl` + `p`                  | Pause dynamic rendering                            |
| `q`                           | Toggle quick rendering, with LCM LoRA if available |
| `n`                           | Random seed value                                  |
| `ctrl` + `n`                  | Edit seed value                                    |
| `UP` / `DOWN`                 | Increase / decrease seed by 1                      |
| `ctrl`+ `s`                   | Save the current generated image                   |
| `ctrl`+ `o`                   | Open an image file as sketch                       |
| `ctrl`+ `d`                   | Call ControlNet detector (replace sketch)          |
| `shift` + `ctrl`+ `d`         | Cycle ControlNet detectors                         |
| `h`                           | Toggle HR fix                                      |
| `shift` + `h`                 | Cycle HR fix scale                                 |
| `shift` + `u`                 | Cycle HR upscalers                                 |
| `shift` + `d`                 | Cycle denoising strengths                          |
| `shift` + `s`                 | Cycle samplers                                     |
| `b`                           | Toggle batch rendering                             |
| `shift` + `b`                 | Cycle batch sizes                                  |
| `shift` + `c`                 | Cycle CLIP skip settings                           |
| `shift` + `m`                 | Cycle ControlNel models                            |
| `shift` + `w`                 | Cycle ControlNel weights                           |
| `shift` + `g`                 | Cycle ControlNel guidance ends                     |
| `shift` + `ctrl` + `g`        | Toggle ControlNel pixel perfect mode               |
| `keypad 0`                    | Restore starting settings                          |
| `keypad 1-9`                  | Load custom rendering preset                       |
| `ctrl` + `keypad 1-9`         | Save custom rendering preset                       |
| `alt` + `keypad 1-9`          | Load custom ControlNet preset                      |
| `ctrl` + `alt` + `keypad 1-9` | Save custom ControlNet preset                      |
| `x` or `ESC`                  | Quit                                               |

_Note_ : "Cycle" shortcuts type will wait for the `shift` key to be released before launching the rendering.

## Installation

Run the Start.bat file and it will create a venv and install a few packages

## Usage

Make sure you have the [automatic1111 webui](https://github.com/AUTOMATIC1111/stable-diffusion-webui) in API mode running in the background and that you have the controlnet extension installed and activated
To start the webui with the API enabled modify the webui-user.bat file by adding ```--api``` after ```set COMMANDLINE_ARGS=```
You also need to make sure the "Allow other script to control this extension" option is enabled in the settings of control net

You can modify the payload.json file for a different prompt, seed or different controlnet model.
When you save the json file the program will use it after the next brush stroke.
in the extra folder there are the names of different controlnet models you may have.
replace this part ```"control_sd15_scribble [fef5e48e]",``` in the Payload.json with a different one from the modelnames.txt
left mouse to draw and middlemouse to erase
press backspace to erase the image.
the program is bound to 512x512 images right now
I may add more features at a later time.
If you want to add aditional models into your controlnet extension you can do so by adding the model folder into the models folder of the controlnet extension.
```bash
    ".\stable-diffusion-webui\extensions\sd-webui-controlnet\models"
```

Windows: [Link to step-by-step Windows installation instructions with screenshots](INSTALL_Windows.md)

macOS: [Link to step-by-step MacOS installation instructions with screenshots](INSTALL_MacOS.md)

Linux: To Do

TLDR; [Web UI](https://github.com/AUTOMATIC1111/stable-diffusion-webui) with
the [ControlNet](https://github.com/Mikubill/sd-webui-controlnet) extension, API mode enabled in settings, and
the [AI Models](https://huggingface.co/lllyasviel/ControlNet-v1-1)

## Configuration

On first launch of the Start file, the script will create the `configs\config.json`, `configs\controlnet.json` and `configs\img2img.json`
configuration files as needed. The ControlNet
available models for scribble and lineart will be automatically fetched from your API and set in configuration.

The `config.json` file handles global interface and script configuration. For these settings, the value used on
application start is the
first value of each of those list:

- `controlnet_models`
- `detectors`
- `samplers`
- `hr_scales`
- `hr_upscalers`
- `denoising_strengths`

The `configs\controlnet.json` or `configs\img2img.json` files can be used to configure the prompt, negative prompt, seed, controlnet
model, etc.
When you save the json file the program will use it after the next brush stroke or when you press `enter`.

A `controlnet-high.json-dist` example configuration file is available for better image quality, at the cost of longer
rendering time.
Use only with a powerful graphics card.

If you want to add additional models into your controlnet extension you can do so by adding the model folder into the
models folder of the controlnet extension.

```
    ".\stable-diffusion-webui\extensions\sd-webui-controlnet\models"
```

### Quick mode / LCM LoRA

The quick mode is able to use a very low number of steps with [Latent Consistency Models LoRAs](https://huggingface.co/blog/lcm_lora) .
To be able to use it fully, please download the LCM LoRAs for SD 1.5 and/or SDXL on huggingface :
- [LCM LoRA for SD 1.5](https://huggingface.co/latent-consistency/lcm-lora-sdv1-5)
- [LCM LoRA for SDXL](https://huggingface.co/latent-consistency/lcm-lora-sdxl)

Save the safetensors files as `LCM_LoRA_Weights.safetensors` in the `./models/LoRA/` directory of your Stable Diffusion WebUI installation.
The name of the LoRA is configurable in the JSON configuration files `controlnet.json` and `img2img.json` with the `quick.lora` entry.
You can also configure the steps, cfg scale, sampler, and LoRA weight used. The default quick setting gives a good quality/performance ratio
with DPM++ SDE Karras and 6 steps. HRFix is disabled when first entering quick mode, but you can activate it if wanted by pressing the `h` key.

### Autosave

The images can be auto-saved after each rendering into `outputs` and `outputs/autosave` directories. The maximum
number of images saved is set by `autosave_images_max` in `config.json`.

This feature can be disabled on start by setting `autosave_images` to `"false"`.

### Multiple ControlNet models

You can update the `config.json` `"controlnet_models"` list to have multiple ControlNet models available. You can then
cycle
between the models using the `l` and `m` keys.

The models set by default correspond to the ones available
on https://huggingface.co/comfyanonymous/ControlNet-v1-1_fp16_safetensors :

- Scribble
- Lineart
- Lineart anime

**Note**: "pixel perfect" preprocessor mode is disabled by default, because it can be detrimental with scribble and
lineart models when used with a resolution > 512. Try out and compare the results.

### ControlNet detectors

You can also update the `config.json` `"detectors"` list to configure the line detection on the generated image. Cycle
between detections
with the `d` key.

The default detectors are:

- Lineart
- Lineart coarse
- Sketch (pidinet)
- Scribble (pidinet)

You can find the full list of supported modules with this URL http://127.0.0.1:7860/controlnet/module_list .

### Custom presets

You can save the current rendering settings by using `ctrl` + `keypad 1-9`, and the current ControlNet settings by using
`ctrl` + `alt` + `keypad 1-9`. Those presets persist in a local `configs\presets.json` file and are available even after the
application
is restarted.

You can load rendering settings with `keypad 1-9`, and ControlNet settings with `alt` + `keypad 1-9`. Not all the saved
fields
are applied by default : the settings that are applied are determined respectively by the `preset_fields`
and `cn_preset_fields` entries of the `configs\config.json` file. For example, if you
want to also apply the sampler value of the preset on recall, add `'sampler'` to your `preset_fields` list.

## Web interface

Alternatively, you can use web interface. Just run `SdPaint\StartWeb.bat`. After creating virtual environment and installing packages it will open interface in your default browser. More in [this instruction](README_Web.md)

## Img2img Experimental mode

Launch the program with `--img2img --source <image_file_path>` to watch an image file for changes, and use it as img2img source.
If the script is launched without `--source` argument, a loading file dialog will be displayed.
The `configs\img2img.json` file is used in this mode.

## Contributing

Pull requests are welcome. For major changes, please open an issue first
to discuss what you would like to change.

Please make sure to update tests as appropriate.

## License

[MIT](https://choosealicense.com/licenses/mit/)