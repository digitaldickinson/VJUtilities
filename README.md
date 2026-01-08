# VJ Teaching Utilities
This is a set of "useful" utlities created for my video journalism teaching. Some are specific to the modules I teach and their assessment. Others are a little more generic. 

**NOTE**:These were all generated with some input from AI to create the code. 

All of these were developed as standalone html pages so that they could be added to our VLE (Moodle) with no local dependencies such as graphics or JS libraries. They do require internet access to online/CDN libraries. Some corporate networks and VLE's may be picky about this, so your mileage may vary here. 

Please feel free to use them as you wish. However, I'd really appreciate it if you let me know if you did. I'd be interested in feedback! Let me know at a.dickinson@mmu.ac.uk

## ScriptBuilder.html ## 
A stand alone webpage that allows a student to visually build formatted script from blocks of content. This was developed for a first-year module, Media Making for Journalists, where the assessment required them to build a simple package to a preset template (hence the basic structure). It works out basic timings and forces the student to include shot descriptions, nat sound information and lower-third information on interviews. It also allows for a basic cutaway in an the interview. 
- Works as a standalone HMTL file (but requires connection to access scripts)
- Student can drag-and-drop blocks for WS, MS, CU, an UNUSUAL ANGLE, Library footage or Full-screen graphic.
- Shot blocks require a shot description and detail of NAT sound before users can export script.
- Shows a live preview of script as student makes changes
- Exports a word document with a script formatted for assessment submission

## Visualscript.html ## 
A more advanced version of scriptbuilder, this is still a stand alone webpage that allows a student to visually build a formatted script from blocks of content. There is no limit to the number of shots. 
- Works as a standalone HMTL file (but requires connection to access scripts)
- As well as the standard blocks, there is now an interview block which included the ability to add multiple cutaways. 
- Most shot blocks have the opportunity to specify overlay graphics if required. 
- Shot blocks can be set to FULL UPSOT/NAT which denotes only NAT sound. No script.
- Temporary lifts in NAT sound (NAT POP) can be shown in script by adding the tag [NATPOP=X:TEXT] where x is the number of seconds you want of nat sound and TEXT is a brief line to point to a specific sound
- Temporary lifts in NAT sound focus on dialouge can be noted with [SOT=X:TEXT]
- Overlay GFX can be defined globally by using the Overlay Graphics check box. But you can time them specifically into a script by using the [GFX:TEXT] tag where the text is the text to show on screen. 
