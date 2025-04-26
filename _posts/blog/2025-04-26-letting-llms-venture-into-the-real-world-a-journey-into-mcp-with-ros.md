---
layout: post
title: "Letting LLMs venture into the real world: A journey into MCP with ROS"
categories:
  - Robotics
  - AI
tags:
  - ros
  - mcp
  - llm
  - claude
  - ai
  - gazebo
  - simulation
date: 2025-04-26 01:52:00
thumbnail: New_Project_nftb4s
---
Recently all the MCP hype had me confused a little. Initially I thought, it's just function calling - we already have , that. Why MCP? But, never got around to actually exploring it.\
\
Until, one post from my network on LinkedIn told me about a real world use scenario where he used MCP to achieve some pretty cool tasks really explained why MCPs are the next thing. Immediately I thought back to this idea I had of using OpenAI API to see and control a Mars Rover prototype at Toronto MetRobotics. I ran a dry test by providing ChatGPT a structured response format and telling it to generate a random set of movements every time it was prompted in that thread. This was before I found out about function calling and schema responses cause I'd never played with the OpenAI API, so I just ended up doing this on ChatGPT. I was delighted to see it could stick to the exact specifications I provided for the control commands. But due to other issues, never got around to doing an actual test on the rover.

![](image_2025-04-26_025902788_odckwg "Orion (the rover) doing some Auton stuff (also cause I'm in the background, 👋🏼)")

Back to the present, by now the Controls sub-team at TMR had implemented ROS on the rover system so I thought instead of one specific thread, I could just make an MCP for the rover and any LLM would be able to see through it's senses and control it too. **BETTER YET-** I make an introspective MCP package for ROS that will be able to create a dynamic MCP server based on all the topics that are being published/subscribed to on the ROS network.

At first I thought - hmm, no way someone hasn't already done this given how epic the community around ROS is (my LinkedIn feed is basically a ROS Community newsletter). But when I searched around for it (even used perplexity) I couldn't find any such thing. Of course, there was [Nikodem Bartnik](https://www.youtube.com/@nikodembartnik) who put ChatGPT on a robot and let it [explore the world](https://www.youtube.com/watch?v=U3sSp1PQtVQ) and a few other people who did similar things. Nothing like a drop-in solution for any ROS system though.

I immediately got to building. I'm gonna skip the technical jargon on this post as I'll be writing other technical posts but in short - it was both simple *and* messy.

Fast forward a day, I had the first working demo of an MCP server that could introspect the ROS topics and provide the last received message on any given topic (I was using Turtlebot3 simulation for this). As my ROS was on WSL2 I had to use a [proxy thingy](https://developers.cloudflare.com/agents/guides/remote-mcp-server/#connect-your-remote-mcp-server-to-claude-and-other-mcp-clients-via-a-local-proxy) to get SSE transport thing working with Claude desktop but eventually got it connected. It was the moment of truth-

![](image_2025-04-26_030457967_dlelyo "Claude seeing stuff from LIDAR scan data")

as you'll see in the screenshot, Claude was able to correctly request the data for the `/scan` **AND** understand it properly!!! At this point, my mind was already blown (even though I kinda expected it to be able to do this). Next up I asked it to count the number of objects it sees and -

![](image_2025-04-26_025955464_mo3frh "Claude desktop counting objects it sees from /scan ROS topic")

would you look at that, it can do that too. This made me even more excited to implement the publishers and prompt it to just do stuff. But it was pretty late, so I went to sleep.

Next night (I function the best during nights lmao), I got back on it and implemented publishers as tools for the MCP server and it was finally time to test it out. First few tries, technical issues popped up and after fixing it all up (more details in incoming technical posts) - here's the result on X (or Twitter, whichever offends you less): 
<https://x.com/mushfiqrrm/status/1915651498853204411> 

As you'll see in the video, it was able to move too!!! Albeit, after having some trouble with data format (this however I believe is a bug). And of course, the robot moved faster than the LLM could keep up, eventually ending in a crash - I believe with the right prompting and setup the community can do some really cool stuff with this. So, right now I'm working on cleaning things up (and basically making it presentable to embarrass myself less than I would right now) to put it out as an open source project.

Stay tuned for that information soon!
