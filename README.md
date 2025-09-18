# llm-playground

Using the green "Use this template" button on the top right, create a copy of this repository under your GitHub account, giving it the same name. Open that repository in a codespace, then run the following commands in the terminal:

### Setup

```bash
bash setup.sh
```

That will install llm, a Python tool for interacting with different LLMs, llm-groq, which specifically connects to Groq, and llm-gemini, which connects to Google's models. You will need to generate an API key at https://console.groq.com/keys. Copy that key, then do the following command in the terminal:

```bash
llm keys set groq
```

You will paste your copied key in the terminal (it won't appear, and you might get a small popup with the word "paste" in grey. Wait a couple of seconds and you'll be able to click on it.). To make sure it worked, run this command in the terminal:

```bash
llm -m groq-llama-3.3-70b "10 names for a pet aardvark"
```

Now create an API key for Gemini here: https://aistudio.google.com/apikey. Copy that key, then do the following command in the terminal:

```bash
llm keys set gemini
```

You will paste your copied key in the terminal (it won't appear, and you might get a small popup with the word "paste" in grey. Wait a couple of seconds and you'll be able to click on it.). To make sure it worked, run this command in the terminal:

```bash
llm -m gemini-2.5-flash "10 names for a pet turtle"
```

You can see all of the models you have access to using llm with the following command:

```bash
llm models
```

### Text Models

The llm library allows us to chat with LLMs, but also to pass things to those LLMs _and_ to log our conversations in a SQLITE database. Here's one way we can start: asking it to summarize the specific contents of a web page that we'll fetch using a tool called newspaper4k that allows us to reduce the amount of tokens we send to the LLM (they have limits):

```bash
python -m newspaper --url=https://cnsmaryland.org/2025/08/02/in-the-deep-woods-a-little-owl-teaches-big-lessons/ -of=text | llm -m groq/openai/gpt-oss-120b "summarize this story in 3 paragraphs"
```

Read the story: https://cnsmaryland.org/2025/08/02/in-the-deep-woods-a-little-owl-teaches-big-lessons/ and then compare that to the summary. Is the summary a good description? Why or why not?

PUT YOUR ANSWER HERE. It was a pretty good description as this pretty much summed up what I initally read in the actual story, which I thought was interesting. 

Now it's your turn: find a story that you wrote or are very familiar with that's published online, and try the same process, altering the command and then evaluating the result.

```bash
$ python -m newspaper --url=https://blackexplosionnews.com/2023/10/23/blog-2023-10-23-panel-featuring-coach-locksley-and-doug-williams-discuss-book-the-rise-of-the-black-quarterback/| llm -m groq/openai/gpt-oss-120b "summarize this story in 3 paragraphs"

```

YOUR EVALUATION HERE. This summary was a very good sum of my story, as it included the basic who,what, when, where and why questions. I wasnt really expecting much initally but it ending up being very cool as it included some of my quotes from my intial story and I thought the overall summarizing from a llm was pretty effective. 

### Restructuring Information

Text models can do more than summarize; they can restructure information to make it more useful. We'll take a recent document describing sanctions of Maryland attorneys (the sanctionsfy25.txt file in this repository) and send it to Llama 3.3 using the `cat` command, asking the LLM to produce JSON data of some of the contents.

```bash
cat sanctionsfy25.txt | llm -m groq-llama-3.3-70b "produce only an array of JSON objects based on the text with the following keys: name, sanction, date, description. The date should be in the yyyy-mm-dd format. No yapping." 
```

That "no yapping" thing? That's one of the ways you get LLMs to stop narrating their output and just give you what you specify.

Now it's your turn: find a short (3 pages or less) news story that contains unstructured text, save it as a text file and drop it into the list of files on the left side. Then create a prompt like the one above that turns some elements of it into structured data. Put your prompt in the space below, and below that tell me how the LLM did.

```bash
cat maryland_bucknell_story.txt | llm -m groq-llama-3.3-70b "produce only an array of JSON objects based on the text with the following keys: player_name, team, position, points, rebounds, assists, quote. No yapping."
```

PUT YOUR EVALUATION HERE. I think this was pretty cool, as it could show us the points, the team, the player, rebounds from just interpreting the story. The LLM was surprisingly accurate at extracting structured data from what was essentially a narrative game recap. It correctly identified individual player statistics like Tafara Gapare's 19 points, Derik Queen's 15 points and 8 rebounds, and Julian Reese's 14 points, even though this information was scattered throughout paragraphs of descriptive text. I also liked that it didnt make up any data and put null at the bottom. 

### Vision Models

One of the options we have with Groq is to use a "vision" model that can describe the information in certain types of images. Let's test that out using an image from this Maryland physician discipline report: https://www.mbp.state.md.us/BPQAPP/orders/D002592202.265.pdf. The specific vision model we're using only accepts jpg, gif and png files, so I've created a PNG from the first page. Run this command in the terminal:

```bash
llm -m gemini-2.5-flash "what is the license number from this image?" -a md_doc.png
```

You should get something like: "The license number in the image is D25922." as a response.

Now it's your turn: find an image _that you would be ok showing to your grandmother_ and make sure it's a PNG, GIF or JPG file. Drag it into the codespace so you can see it in your list of files, then change the command below so that it refers to your image and asks a different question. Edit the README to do that so I can see your work. How did the LLM do?

```bash
llm -m gemini-2.5-flash "What are all of the colors included in this photo" -a colorphoto.png 
```

PUT YOUR EVALUATION HERE. The model performed exceptionally well at identifying all the colors present in the photo. I was curious to test whether it could accurately recognize and distinguish different colors within an image, and it exceeded my expectations. The vision model not only identified the primary colors but also picked up on subtle color variations and gradients.


### Audio Models

Another option is to use models that transcribe audio. Listen to [this mp3 file](https://dare.wisc.edu/audio/south-carolina-desegregating-edisto-state-park/) from the Dictionary of American Regional English project at the University of Wisconsin. Click the download link and save the .mp3 file to your computer, then drag it to the file to the list of files on the left. Then, using llm, have the `whisper-large-v3-turbo` model from Groq provide a transcription using that file name.

```bash
llm -m groq/whisper-large-v3-turbo -a SC068clip.mp3 
```

How did the LLM do compared to the original transcript? 

PUT YOUR EVALUATION HERE. The Whisper model provided an accurate transcription, capturing not only the words but also maintaining proper punctuation and paragraph breaks. 

### How You Could Use AI

Thinking about news archives, write a few examples of how you might use LLMs to help (beyond writing code). Be specific: don't say that you'll use it to accomplish a larger goal. Instead, say how it could perform or improve specific tasks. I encourage you to think big.

PUT YOUR ANSWERS HERE.  You can use LLMs for news archiving by doing document classification and tagging, this would include automatically categorize thousands of archived articles by topic, extract and standardize metadata like publication dates, bylines, and source information, and identify and tag articles containing specific entities (politicians, companies, locations). 
Also the use of LLMs to analyze the subtle emotional language and cultural attitudes embedded in decades of news coverage from the archives is something I think would be a good use. For example, you could track how the language used to describe mental health issues evolved from the 1950s to today, revealing shifts in social stigma and understanding. This could uncover fascinating stories about how societal attitudes changed over time, which events triggered shifts in public discourse, or even identify bias patterns in how different communities were portrayed across different eras. 

### Finishing Up

When you are finished, add, commit and push your work to GitHub and submit the URL of the repository in ELMS.