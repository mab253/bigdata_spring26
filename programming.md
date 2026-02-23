# 🤖 Spark Programming Assignment 

**due: March 8, 11:59pm** - 

## 🕸️ the common crawl  

For this assignment, you will use a **Docker container** to run a local instance of Spark and PySpark. You will use that environment to download and analyze segments of the [Common Crawl](https://commoncrawl.org/), a dataset created by a non-profit that programs bots to regularly index massive portions of the web.

🐳 We are using Docker so that everyone can be on the same installation and notebook environment for Spark, even though we all have different machines. [Docker containers](https://www.docker.com/resources/what-container/) package up all we need in terms of dependencies, tools, etc. to run the Spark application. It means we don't have to worry about everyone's unique OS or separate Java installation; it's like a lightweight virtual machine, and environment within your own computer that matches everyone else's who downloads the same "image" (package of specific instructions) from Docker.

For this assignment, you will need to:
- download [this `ipynb` notebook] that I've created for class, somewhere in your local filesystem so you know where to find it
- download (free) Docker desktop - [choose the right download](https://www.docker.com/products/docker-desktop/) for your operating system
- once you have Docker desktop on your machine, you will need to [open up a **shell** or **terminal** window](https://www.youtube.com/watch?v=m2YKlRaO26A).

With the shell/terminal open, type this command exactly and press Enter:
```
docker run -p 8888:8888 -p 4040-4050:4040-4050 jupyter/pyspark-notebook
```
You should see a bunch of texts start to fill up the logs in the terminal! Scroll down until you see something like:
```
 To access the server, open this file in a browser:
        file:///home/jovyan/.local/share/jupyter/runtime/jpserver-7-open.html
    Or copy and paste one of these URLs:
        http://7e2417056d03:8888/lab?token=4dd92aed589d51dcf0fe6e26a611e155059ec5926952581e
        http://127.0.0.1:8888/lab?token=4dd92aed589d51dcf0fe6e26a611e155059ec5926952581e
```
Despite what the instructions say here, you only want to copy/paste the last one (`http://127.0.0.1:8888`) into a browser window. Alternatively, you can go to `http://localhost:8888` in a browser window, and you will need to enter the `token` that you see in the above text from the terminal, and create a password for yourself. Either way works!

👀 Ultimately, we want to see a Notebook environment, like this:
<img width="1227" height="679" alt="Screenshot 2026-02-23 at 16 13 28" src="https://github.com/user-attachments/assets/6e5f905c-5ca9-41f9-8819-8bf354680ddb" />

Next steps:
- Next to the blue "+" plus sign, you'll see a little button with an "Up" arrow that says "Upload Files" once you hover over it (shown in red in my screenshot above).
- Click **Upload Files** button to add the assignment `.ipynb` notebook into the Docker container file system, the one you already downloaded. You should now see it listed on the left.
- Double click on that assignment notebook, and you should see the assignment ready to go! 
<img width="1227" height="679" alt="Screenshot 2026-02-23 at 16 23 18" src="https://github.com/user-attachments/assets/7900234a-3a70-4e6f-ad0c-d65910d4ccf9" />

- When you are ready, go through the cells in the notebook, writing either Python code or text, depending on what each cell asks you to do.
- You may want to consult the Codecademy course as you go through - most of the material in the questions was covered there.
- This is not an exam (it's open book), and you are allowed to ask questions about the assignment on Discord. (You are **encouraged have collaborative discussion** in the #code-questions channel to help each other out.) Please make sure that **your final work is your own code and words**, make [citations in comments in your code](https://github.com/mab253/bigdata_spring26/blob/main/citations.md) if you turn to other resources (especially _every instance of GenAI_), and you will also be asked to list the sources you used at the end of the notebook.
- There are also a few questions marked like this: ✍️ and they ask you to "Double click and answer in full sentence format." These are questions asking for your **original writing,** no external quotes or generated content here. Please answer the questions in short paragraph format.
- Happy coding!

**💥 IMPORTANT NOTE:** if you need to save your work and come back to it later:
- Make sure you **SAVE** often with the little disk icon.
- But this only saves to the Docker container - which is like a mini-machine with its own processes in our larger machine, so when we close the Docker container, we don't have access to those files until we open it again.
- To get a local copy, I also recommend regularly choosing **File -> Download** to keep your notebook progress locally, not just in the Docker.
- To stop the Docker container, in the terminal/shell you can type `Ctrl - C` to halt the process.
- You can then type `docker ps` to see the list of active containers - it's probably a blank list if you pressed `Ctrl - C`
- To restart your container:
  - You can type `docker ps -a` to list _all_ the containers, even ones you closed.
  - Find the most recent one that you closed with the image name `jupyter/pyspark-notebook` - check out the `CONTAINERID`
  - You can type `docker start CONTAINERID` (substituting CONTAINERID for the number/letter combo you found)
  - It should just print out the ID back again if it's working!
  - Now go back to `http://localhost:8888` - you'll see a web page asking for your token
  - Back in the terminal: `docker exec CONTAINERID jupyter server list` - make sure to sub for the real container ID
  - The terminal should print the `token` you need, you can copy/paste that into the website prompt, and you should see your old notebook progress again, from the last time you **Saved**
  - _Alternatively_, you can follow the intro steps from this document, and start a **new** Docker container, and just repeat the Upload Files step with your saved progress.
  - If you want to stop a Docker container, the command = `docker stop CONTAINERID`
