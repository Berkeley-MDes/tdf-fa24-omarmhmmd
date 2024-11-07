# Week 11
WIP

# Week 10
This week I got my hands dirty wirh ZeroWidth. My original ideas were to compile all of Rumi's poetry and use the AI to bring out insights not readily avaialable to non-farsi speakers. I was able to find this repository on the internet archive:

![November_04_2024_08h23m27s](https://github.com/user-attachments/assets/f0734c8f-b878-400b-bdef-cd03bffeed03)

However, the chunk sizes became extremely huge and unmanageable. I am thinking to play around with this for the final project and see how I can manipulate the chunks even at a small scale to investigate line by line poems, like we investigate line by line code. 

I was able to support my own chatbot, that digs into my CV. 

![November_04_2024_08h22m02s](https://github.com/user-attachments/assets/977bc0e8-baeb-4789-8454-abbc978ce3f7)
![November_04_2024_08h21m48s](https://github.com/user-attachments/assets/111fe6dd-7af6-41f5-8411-40b6f195e787)
![November_04_2024_08h21m29s](https://github.com/user-attachments/assets/1c19c42b-2157-4062-a50e-0df64f1dad93)

The tutorial TJ created was super instrumental in figuring this out, and I am eager to understand how to create your own knowledge bases to inform the outcomes. 

# Week 9
This week was the final push for our project. Abdi did an amazing job getting the casing for all of our components together. We had to create a casing that would hold our sensors, LEDs, and OLED display fit all snug and tight. 

![IMG_9190](https://github.com/user-attachments/assets/b242e565-b078-41d6-aa12-1d6cb295353c)

After getting all the pieces assembled, I wish there would have been more time to explore PCBs and actually soldering all of the connectors together. This "prototype" mode had to stay static and any type of movement affected the pins resulting in need for debugging. 

![IMG_9195](https://github.com/user-attachments/assets/08e805a3-998c-4c54-b441-4fc2851cee60)

The OLED display reads "Israel has killed 42718 Palestinians" with that number being updated everyday. 

https://github.com/user-attachments/assets/1509f138-1254-4092-ba3c-5f5901a5b780

The Palestinian Death Counter project, developed by Joseline Chang, Omar Osman Mohammad, and Abdi Hamisi Ambari, seeks to evoke an emotional response by displaying the real-time death toll in Gaza. Utilizing the Photon 2, OLED screens, ultrasonic sensors, LEDs, and an API from Tech for Palestine, the system integrates 3 inputs and 3 outputs, surpassing the original project requirements. The objective is to create a powerful, unsettling technological artifact that symbolizes the ongoing genocide in Palestine, represented through abstract visuals like flickering LEDS in the colors of the Palestinian flag and dynamic counter displays on OLED screens. Team roles are clearly divided, with Omar focusing on the API, Joseline handling the physical technology assembly, and Abdi coding the LED displays. The project emphasizes sophisticated design, robustness, and skilled execution. Through a combination of symbolic components and indirect user engagement, the team hopes to provoke deep thought and emotional awareness in viewers. Expected outcomes include a finalized system that effectively conveys the rising death toll in Gaza through abstract but powerful visual elements.

---

After creating the final object, we immediately learned that the placement of the piece should not be a desktop object, but adhered to the wall. The display can be magnified, and lights used more intentionally to create a device that exists at the human scale, rather than a tabletop item. 

Next iterations would explore these concepts of placement and human interaction. 

# Week 8
Tasks for this week included understanding how Photon 2 deals with APIs and JSON parsing. It was a CHALLENGE. After reviewing [Tommy's super helpful tutorial doc
](https://github.com/Berkeley-MDes/tdf-fa24-TommyJing0/blob/main/Week%207.md), I was able to understand how the Particle console deals with API integration. 

![October_21_2024_17h34m49s](https://github.com/user-attachments/assets/92a5d261-280d-40f5-9e33-ab87a762e568)

In the console, it is super simple to set up a custom webhook using an existing API. Simple form input, set the request to a "GET", and use the ``` Particle.subscribe() and Paritcle.publish() ``` functions with custom parameters to get the API going. 

A simple response looks like this in code:

``` C++
  void setup() {
  display.begin(SSD1306_SWITCHCAPVCC, 0x3D);  // 0x3D is the I2C address for most 128x64 OLED displays

  display.clearDisplay();

  String data = String(10);
  Particle.publish("getDeathTotal", data, PRIVATE);
  Particle.subscribe("hook-response/getDeathTotal", myHandler, MY_DEVICES);
}


void myHandler(const char* event, const char* data) {
    display.setCursor(0, 0);
    display.setTextSize(1);
    display.setTextColor(WHITE);

    // Parse the JSON response to extract the "last_update" field
    StaticJsonDocument<512> doc; // Increased size for larger JSON
    DeserializationError error = deserializeJson(doc, data);
    
     const char* lastUpdate = doc["gaza"]["last_update"];
     int totalKilled = doc["gaza"]["killed"]["total"]; // Directly assign to an int
     display.print("Total Killed:");
     display.print(totalKilled); // Print the total killed number
    
     display.display();
}
```
I initially was using a dataset with total deaths, which had almost 50,000 entries. The Photon had an extremely tough time parsing this data. I found a much [simpler API](https://data.techforpalestine.org/api/v3/summary.json) that gives data as a simple JSON object. 

It looks like this:

``` JSON

  "gaza": {
    "reports": 380,
    "last_update": "2024-10-20",
    "massacres": 3709,
    "killed": {
      "total": 42603,
      "children": 17029,
      "women": 11585,
      "civil_defence": 85,
      "press": 177,
      "medical": 986
    },
    "injured": {
      "total": 99795
    }
  },
  "west_bank": {
    "reports": 380,
    "last_update": "2024-10-20",
    "settler_attacks": 1486,
    "killed": {
      "total": 728,
      "children": 159
    },
    "injured": {
      "total": 6471,
      "children": 1052
    }
  },
  "known_killed_in_gaza": {
    "records": 34344,
    "female": {
      "child": 4936,
      "adult": 6643,
      "senior": 791
    },
    "male": {
      "child": 6419,
      "adult": 14347,
      "senior": 1208
    }
  },
  "known_press_killed_in_gaza": {
    "records": 177
  }
}
```

The data was successfully displayed on screen, however a small text bug has appeared. I assume converting this to a string first will handle the issue. 

![IMG_9131](https://github.com/user-attachments/assets/da8a177e-4ce1-4b4b-a0f6-8d408d89203a)

We are currently putting all of the inputs and components together. 

# Week 7
For this week, my teammates and I, Abdi & Joseline, got to work on brainstorming concepts for Project #2. I did not know think that we would jump straight into group project mode without personal time to experiment and explore, so scratch last weeks concept about MakkahTV. I wish there would have been time to explore something more personal and research based, rather than an amalgamation of different components into one project. 

Our first concept was introduced by Abdi, who stated he wanted to relate our work to the global war conflicts occuring around the world; something more intentional and thoughtful versus a simple gadget. I knew there exists a an org called [Tech for Palestine](https://data.techforpalestine.org/), and we also stated we wanted to explore with an API, so I knew exactly where the project could land. 

After an initial brainstorm we introduced a lo-fidelity poetic object, that uses the API and simple LEDs to abstractly represent each Palestinian death recorded. As of today, 42,894 Palestinians have been murdered by Israel. Conceptually, if we flashed an LED every second in regards to each death, that would take 42,894 seconds, which would result in 11 hrs 54 min 54 sec of blinking lights. In order for the process to complete, a viewer would have to "watch" this object for almost 12 hours. Since that's an unrealistic experience, the magnitude of the project lies inherently in its absurdity. Here is a potential schematic: 

![image](https://github.com/user-attachments/assets/90f6bc74-5db0-4e49-ae5d-3eaea2774af7)

During critique, only with the professor, we were told we needed three inputs. It turned into simple gestures of "add a sensor that begins the loop" or a "button" to get things started. Immediately the criticality of the concept was dumped in order to fulfill "requirements"; philosophically I believe that this unneccesary grafting of technology has placed us in the shallow depths of the field currently. What do we teach designers when we ask them to "use up" as much of the design possibilities as possible? Adding more features for the sake of adding more features is a critical piece of emerging technologies that I think we should be thinking about and applying to our work. 

For now we will be adding an "on/off" button, a dial for "speeding up" the process, and a sensor that recognizes a viewer is present to restart the procedure over again. 

This is our [working doc](https://docs.google.com/document/d/1-4qWT8x8U70rVELNhtWvcTt5QF2wPjlXmcCdbx9gS9E/edit) for now. 

# Week 5-6
Last week I presented some design work in Brooklyn, so I missed class! I have some experience with microcontrollers, however. This is a project I worked on during my last time at UC Berkeley: Qiblah https://omarmhmmd.notion.site/Qiblah-3f57157cfb984576a9d40ff4b4739832.

It uses a bunch of components, leds, a compass, a touch capacitor, to make visible the direction of the qiblah through a physical interface. It took me about three weeks to complete.

![image](https://github.com/user-attachments/assets/6ee16a69-c828-4f7b-abd3-d363743d0dce)

Right now, I'm not having much fun with the particle/photon. I don't think it's as intuitive as a raspberry pi and arduino, and definitely not beginner friendly. It's also way more "engineering" than a "design" tool. I'm confused on its usage in class. I was able to get the touch capacitor to work pretty easily though.

![Photo on 10-3-24 at 1 17 PM](https://github.com/user-attachments/assets/b8020e50-61dd-4c10-8811-d317df5bdfc6)

I have some ideas that I've been wanting to work on with microcontrollers for a while. One of them is a concept called MakkahTV, or MTV. https://www.are.na/omar-mhmmd/makkahtv.

![October_03_2024_13h27m15s](https://github.com/user-attachments/assets/44a24870-aa25-4908-9ebd-8df3f3841108)

Makkah has been live streamed on TV since I can remember watching a live satellite transmission at my grandparents as a child. This livestream has evolved to the internet as a YouTube live stream with a live message board. The holy city of the Prophet [SAW] Madinah is also live streamed. Cultural objects pertaining to Islamic prayer times have often taken on the form of the Ka'aba, the main center of focus in these live streams.

![image](https://github.com/user-attachments/assets/cf501503-66c4-4eea-b62e-d5357bfd9703)

I imagine a meta-object, a ka'aba "televsion" whos walls are the screens of the live stream, depicting itself. 

![IMG_0031](https://github.com/user-attachments/assets/15265d24-23ff-4e4d-a8b7-4acfd02cd917)

In this sketch, a 3D printed cube houses small screens that are connected to a youtube live stream, through a rasberry pi that is houses in an acrylic base. This use of material slightly emulates the actual build and materiality of the ka'aba, without being a 1:1 reference. I am conducting research on how the Photon can handle video, or if a need for a rpi will become necessary. I'm hoping to use my time in studio foundations to create the 3D and laser cut elements of the model. We shall see.

---
# Week 4
Grasshopper is EXTREMELY popwerful. Before project 1, I had never worked with grasshopper, relying primarily on sketching and static rhino drawings to explore form and object properties. Exploring even the basics of grasshopper has opened my eyes to physical design and the overall design process. A simple garbage on the side of the road appears differently to me now; what methods and computation could I design this garbage can with in grasshopper that would produce novel ideas through the design process. 

My first attempt at a parametericized design in grasshopper. I was mind blown at the possibilities this afforded. 

![September_12_2024_15h45m13s](https://github.com/user-attachments/assets/1292295e-0c0f-4e42-b5e4-30f83bc62335)

After many iterations and evaluations, my initial assumptions and ideas for to the project shifted. Grasshopper enabled me to break my idea down, realize it was "incorrect", and allow me to proceed to a successful outcome. 

![September_12_2024_15h48m22s](https://github.com/user-attachments/assets/a761c65e-8f52-467f-9e7a-e402a59da72b)

The project for this week was really great when it came to the software and the process, however the execution and timing was a little rough. I really believe we should have been given more time to explore, and perhaps had a critique of round 1/version 1 printouts. I'm hoping that future assignments will be more organized and give students ample time for the design process and the production process. 

---
# Week 3
I think I was feeling pretty overwhelmed after weeks 1-2. The Wednesday start, the Monday off; everything felt rushed and intense! Things have relaxed and I'm finding a rhythm between all my classes. I've had some time to finally think about this course and what I want to get out of it. My initial sketches and ideas for project one are below. I think this is the "final" stage of difficulty...but let's see how far I can get! It's a challenge using grasshopper, NOT GUNNA LIE. 

## Rehal رحل
Visual research -> https://www.are.na/omar-mhmmd/rehal

![September_12_2024_15h50m19s](https://github.com/user-attachments/assets/4cf77dc6-28ec-47b9-9b37-f013033d5238)


I've been thinking a lot about rehals, or "book stands", since my aunt gifted me one from Turkey when I performed a khatm-e-quran (full reading of the quran) at her home. It's a big one, with many intricate designs carved out of it. This opened my eyes to the rehal as an islamic design object, a functional device that evokes beauty, ultimately to the Divine. The rehal is a bookstand, but it's main use is to never allow a Qur'an to touch the ground. In masjids, the tradition of sitting on the ground, with no furniture except chairs for the elderly and injured, has remained a constant.

In order to read the Qur'an a stand that can be folded, put away, and re-emerge simply was required. I need to do more research on the history of this object, however the word "rehal" in its verb form in Arabic means "to saddle, to mount"; referencing camel saddles, the bookstand rehal appears a saddle for books. The rehal today is traiditonally carved from one piece of wood, embedded with chiseled joints, and ran through a saw to "open" the joints and fold. This video has 1.9m views from a year ago; perhaps I'm not the only one who's thinking about them. -> https://www.youtube.com/watch?v=ENoiyqCeHlk

In people's home, at the masjid, in prayer spaces, one comes across a multide of rehals in different sizes, shapes, and craftsmanship. I've wondered what a 3D printed rehal, or CNC'd rehal, or just a 3D rendering would look like, and what the software and technologies might add or takeaway from it. I've never used grasshopper, but I think the software enables a great challenge in investigating quickly and iteratively different shapes, sizes, angle openings, and orientations of the object. I'm especially interested in embedding islamic patterns, even if simple, using grasshopper; I think that type of design is one of its strengths!


![September_12_2024_15h45m13s](https://github.com/user-attachments/assets/d747c775-e2cb-4cdd-bcfe-75f198bdf7eb)
<br>
This is a gif showingmy progress in designing one in GH. I'm only able to explore different widths really, but I'm hoping for my next step to be able to explore angle openings. 
<br>
<br>
![September_12_2024_15h48m22s](https://github.com/user-attachments/assets/d13856e9-3f4b-4329-8c62-3c786d18ee95)
<br>
The angle openings and position of the center joint on the z-axis can radically shift the form of the shape. 
<br>
<br>
![original_e42a15ae4cd89db34f2d8db61b38f42a](https://github.com/user-attachments/assets/e4143d7f-0893-48ce-a267-ad9b76bf781d)
![original_948a7e6875e751514bd4ee4698d7a83c](https://github.com/user-attachments/assets/806f03c4-b890-4218-b1d3-dd6e9cda94fa)
<br>
Notice the elongated legs which creates a wider angle between the base legs.
<br>
<br>
![September_12_2024_15h49m20s](https://github.com/user-attachments/assets/ac886e32-f788-4f0d-a585-1683be599d72)
<br>
My grasshopper code so far contains duplicates, which I don't think is the best way of doing things...I'm hoping to get the some guidance today in class on this!
<br>
<br>

---

# Week 2

### On the syllabus
* I feel like the syllabus is very overwhelming
* Too many links without enough context, or maybe it's just early in the semester but this is how I'm feeling
* Tutorials shared with students must be placed within a contextual framework
* Provide a historical or contexual framework to build an assignment to learn from
* Give a *reason*, not just the sake of the tutorials
