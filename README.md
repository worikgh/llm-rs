# A Package to Interact with Large Language Models

This is only interacting with  [OpenAI's language models](https://api.openai.com/v1/)

## The Command Line Interface `cli`

There is a library that exposes the various endpoints and a command line binary (`cli`) to use it

To use: `cargo run --bin cli -- --help`

```
Command line argument definitions

Usage: cli [OPTIONS]

Options:
  -m, --model <MODEL>                  The model to use [default: text-davinci-003]
  -t, --max-tokens <MAX_TOKENS>        Maximum tokens to return [default: 2000]
  -T, --temperature <TEMPERATURE>      Temperature for the model [default: 0.9]
      --api-key <API_KEY>              The secret key.  [Default: environment variable `OPENAI_API_KEY`]
  -d, --mode <MODE>                    The initial mode (API endpoint) [default: completions]
  -r, --record-file <RECORD_FILE>      The file name that prompts and replies are recorded in [default: reply.txt]
  -p, --system-prompt <SYSTEM_PROMPT>  The system prompt sent to the chat model
  -h, --help                           Print help
  -V, --version                        Print version
```


When the programme is running, enter prompts at the ">".

Generally text entered is sent to the LLM.

Text that starts with "! " is a command to the system.  

## List of Commands
|Command| Result|
|:---|:---|
|! p| List internal settings|
|! md| List available models|
|! ms <model name>| Select a model|
|! m <mode>| Set mode. |
|! cd | List context.  In chat mode context is maintained |
|! cc| Clear the context.  This starts a new chat|
|! k <int>| Set max tokens for completion models
|! t <int>| Set temperature for completion models
|! sp [`<prompt>`]| Set or display the `system` prompt.  This only really makes sense in chat after `! cc`  |
|! mask <path>| Set the mask to use in image edit mode.  A 1024x1024 PNG with transparent mask
|! ci | Clear stored images.
|! ?| Display options|
C-q or C-c to quit.



## Modes

The LLMs can be used in different modes.  Each mode corresponds to an API endpoint.

The meaning of the prompts change with the mode.

### Completions

* Each prompt is independent
* Temperature is very important.
* The maximum tokens influences how long the reply will be

### Chat

* Prompts are considered in a conversation.
* When switching to chat mode supply the "system" prompt.  It a message that is at the start of the conversation with `role` set to "system".  It defines the characteristics of the machine.  Some examples:
  * You are a grumpy curmudgeon
  * You are an expert in physics.  Very good at explaining mathematical equations in basic terms
  * You answer all queries in rhyme
.

### Image and ImageEdit

Generate or edit images based on a prompt.

![_Example Image_](examples/A_line_drawing_of_marilyn_monroe.png){:width="100px"}

Enter Image mode with the meta command: `! m image [image to edit]`.  If you provide an image to edit "ImageEdit" mode is entered instead, and the supplied image is edited.

If an image is not supplied (at `! m image` prompt) the user enters a prompt and an image is generated by OpenAI based n that prompt.  It is stored for image edit.  Generating a new image over writes the old one.  

**Mask**  To edit an image the process works best if a mask is supplied.  This is a 1024x1024 PNG image with a transparent region.  The editing will happen in the transparent region.  There are two ways to supply a mask: when entering image edit, or with a meta command

1. **Entering Image Edit** Supply the path to the meta command switching to Image Edit: `! m image_edit path_to/mask.png`
2. **Using the `mask` Meta Command** The mask can be set or changed at any time using the meta command: `! mask path/to_mask.png`

If no mask is supplied a 1024x1024 transparent PNG file is created and used. 

