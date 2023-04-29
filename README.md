# Sally.py

A discord bot that combines a LLM + SD to make a Chatbot which can send selfies

## About this project

I made this project after I saw a youtube called DeSinc release a [video](https://www.youtube.com/watch?v=KM4a7RGG270) on his youtube channel. It previewed his Chatbot that responds to people in his Discord Server which could even send selfies. I actually always liked to have my own chatbot, but thinking about it I have never really thought about running my own LLM. Looking through his [repo](https://github.com/DeSinc/SallyBot) and the ways he used to get a LLM to work (Text Generation Webui) I realized that it is actually really easy to get a LLM to run on your system. So with my new knowledge and a few tricks from his c# code I made my own implementation of "Sally" the LLM Discord Chatbot. Because I am not too far into learning C languages as of right now I made up most things my own and the few things I was understanding of his c# code I implemented in my own style. I currently also support OCR so the bot can read text in images that you send it. I also linked his repo above which explains how you can set SD and Text Generation Webui up.

## Setup

- Install Python (I use 3.10.10 right now) and install all required packages like so:
- ```pip install -r requirements.txt```
- Next you need to edit the bots_config.py file. It contains all config stuff I use for sally to run and also some variables which I didn't want to have in the main file because it would be pretty messy. Rename the .env.example to .env and add your bot token and white/ blacklisted IDs in there, in the bots_config.py select if you want to use SD or not, edit the bot's displayed activity, change the memory amount (with memory I don't mean RAM here but rather the amount of messages in the discord channel which it will save to gain context about the situation, too many can cause long waiting times on the first message and or rate-limits and also the bot getting confused. I recommend 10-20, but you can play around with it), and for everything else in there, only change it if you know what you're doing and if you have questions I would love to answer them to you.

## Usage

- Now you can run the main script like so:
- ```python main.py```
- If everything worked, you config should be printed out to you and the discord bot should start running, now you need to run the oobabooga webui like so:
- ```python server.py --model ozcur_alpaca-native-4bit --wbits 4 --groupsize 128 --extensions api --notebook --listen-port 7862 --xformers```
- You can add any other model with whatever parameters you want, but you need to keep these options as they are: ```--extensions api --notebook --listen-port 7862```

## System requirements

As mentioned earlier I was never too sure on how I am able to run a LLM on my system, this is due to VRAM limitations. DeSinc's Repo also mentions this but I thought I would add my own experience. I use ozcur alpaca native 4bit model, why is this important? Well it is quantized, this means it runs on 25% of the VRAM which is required by the same 7B (I think the original is 7B) model. It has lower persicion but I don't think it is tooo important to have a full persicion model if you still have a high amount of parameters. Just to know what you're dealing with, I run this code currently on a 3080 (10GB VRAM) and I can use it on about 9 GB of VRAM (both SD and TGW). If I up the SD image resolution a bit then I can also max my VRAM out. The current SD parameters are for my GPU to run both at the same time. My conclusion therefore is, having 10+ GB is very good, it can enable you to get a higher resolution in SD or it could enable you to use a different (maybe non quantized) model witch TGW. But if you have less than 10GB VRAM then you should consider:

- [Changing settings for low VRAM usage in TGW](https://github.com/oobabooga/text-generation-webui/blob/main/docs/Low-VRAM-guide.md)
- [Same thing for SD](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Optimizations)
- Changing the resolution in the bots_config.py for SD
- Using a different model for SD (not too sure about this)
- Not using SD at all (for that set stable_diff_is_used to false in bots_config.py)
- Using a different model for text generation (I would rather not use SD instead of downgrading my model for text generation)
- Use a cpu based Text Generation model (very slow and makes pc go brrr) like Dalai. It is implemented for DeSinc but not for me (yet, I might add it aswell soon, I just didn't get it to work properly so far) but it is really a pain in my opinion.
- Upgrade your GPU (maybe you wanted to anyways, this might just be another reason to do it :> )

Also, if you are really determined, you might be able to use an API for a text generation model. Things like chatGPT already have API's (which you need to pay money for) which you could use with some edited code and research for this project too. I used to have a bot that was using <https://beta.character.ai>. The problem with that was that they didn't have an API so I made request to the site and did some fancy octet streaming stuff to get a response. It was really quick and working awesome until at some point they started to change some code in their website and then it stopped working and I kind of never got back to that project again. It could also be way harder though to give the bot the same sense of context and everything but their model is probably even better than mine + you are able to give your bot a character with background info (which I don't know how to do with this).
