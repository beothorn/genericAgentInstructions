# Setup 

First ask the user what is the project. Ask if the user level of knowledge on software development. This will be used to fill the audience section. 

Ask who will use the system, how many, where will it run, if it's made of many parts.  
Ask expectations of performance, costs ifa applyable, if it is an experiment, a final product, how will it be distributed, how are the plans for it evolve, steps or phases planned and so on. Ask architecture until you are able to have a picture and make decisions on what tech will fit best for the project. You will put this in the tech docs. 

If the user has knowledge, ask about the language, frameworks, build system and so on.   
If the user is not knowledgeable you can evaluate what are the best choice.  
User decisions take priority always. 
Search online for the popular way to organize code on the given language, the most popular test framework and building tools.  
Look online the most popular setup for Ai setups, with skill folders, prompt folders, scripts folder and so on. Add to the architecture. 
Search how to debug from the command line. This is very important because the agent will debug the system. To investigate issues, it will create unit tests or small scenarios and debug it.  
For Java for example, look for jdb debugger skill or javaflame. Write skills for debugging, changing the log level and analyzing the logs. For example, for things running on devices, such a android, you would need to use adb. 
Write the Agents and all other folder and files related to the AI setup. Fill all missing sections on this Agent and whatever other changes are necessary.  
Commit as Ai initial setup. 
Then setup the project according to the language and other choices from the user.
If the user needs to install things, suggest it to the user. Give a list of things in a table and why they are needed. Offer to install if needed.  
After all this, delete these setup instructions from the Agents.md.  

# Audience 

## Dev or Client 

Fill after setup. Here goes the description of the person using the AI, so the AI knows what to ask and what language to use (technical, business oriented, design?)

## Userbase 

Fill after setup. Here goes the actual final user description.  

# Tools

Fill after set up  
Create an aibin folder and add to git ignore. Put executables that you download here. Do not put scripts here, put the scripts on the appropriate folder for AI. 

# Skills 

Fill path after setup  

# Architecture 

Fill after set up

# Security 

Never commit passwords, keys or anything sensitive. Prefer having them as env vars. Create a folder secrets and put it on git ignore. Ask the dev to configure env vars and if really needed put secrets on this file. Avoid at all costs reading the secret. Ask the user to edit a file with the password or key on the secrets folder if needed. If the user wants to push a secret anyway, advise they not to, but if they insist (could be that is for tests, for example) give a warning and do it, 

# Code

These instructions must be adapted to the programming language chosen  

Commit often, in small self contained steps. Always us a good descriptive title and a description of what you done, why and who asked for it and how was this asked.  
Rely on git blame to understand why a code is like it is. also use git log to understand code motivation.  
If you need, revert commits, cherry pick or create feature branches.  

Keep everything simple. Less code is better.  
The secret of good code is isolating responsibilities. Good code works like many small programs you can reason about by themselves.  
For this, pay attention to break down code in folders, first per domain and then per layers.  
example instead of ./database/payment ./screen/payment  
prefer ./payment/database ./payment/screen  

Add as many folders needed to separate concerns in a way easy to quickly identify where a function comes from  
The folder depth is very important to easily navigate the code.  
Common code need to "bubble up" to the highest common folder.  
example:
./FooDomain/Foo.code ./FooDomain/Bar/BarUsesFoo.code ./FooDomain/Baz/BazUsesFoo.code (Domain is just an example, please avoid generic suffixes when naming)  
So here Foo code needs to be on the highest folder. In case Foo is a functionality with many code files, the folder "bubbles up"  
example:  
./FooDomain/FooThings/Foo.code ./FooDomain/FooThings/FooHelper.code ./FooDomain/Bar/BarUsesFooThings.code ./FooDomain/Baz/BazUsesFooThings.code  
It "bubbles up" only as high as needed, but not higher. This way it is contained only where used, and things that are often used goes very high on the folder hierarchy.  
If you have A, B, C on a folder, and A and B both depend on C, keep C on the parent folder and create a new one for A and a new one for B. So ./A ./BCode/B ./CCode/C
Folders should hold code that is as independent as possible. This way when inside a folder, I can focus on code on that folder.  
The goal is to know that if I am in a folder without child folders, I can read it or maybe navigate up to read code related, but avoid navigating sideways on the folder structure.  
There will be cases where this is not possible, then try follow the principle of self contained as possible.

Code should be easy to find and to read.  
Prefer immutability, except when performance is affected.  
Prefer less complexity, self contained functions, imutable variables, bigger meaningfull names over small naming (unless it is a global function used everywhere).  
Keep code declaration and usage close when possible.  
Keep code independent and flexible to be moved. That means avoid complex inheritance, prefer composition.  
Break responsibilities in different functions and files.  
Avoid complexity, count the number of things one need to remember when reading a piece of code (like variable states, if a condition was met or not)  
Prefer strategy instead of decision taking, or in other words, put conditionals as early as possible.  
Code should have as little conditionals as possible, and conditionals should be evaluated as soon as possible.  
Extract variables and give good names and types. Example (in pseudocode): Instead of
```
store(5);
```

prefer

```
int currentUsersCount = 5;
store(currentUsersCount);
```

Try to keep the code width small. Prefer early returns using if-return at a function start instead of if-else. Example: Instead of

```
if(x != null && x.specialCondition()) {
    do the thing
} else {
    log and return 
}
```

prefer

```
if(x == null) {
    log and return
}
if(!x.specialCondition()) {
    log and return
}
do the thing
```

Think it like **preparing the terrain and make sure you have all needed to do something**.
example:
Instead of passing a boolean with value shouldExecute and doing an if on it, evaluate if should execute and depending on it, call a different function.  
Avoid magic, where magic is code that you cant trace where the values or the call comes from.  
Example of magic is reflection, where you need to know runtime values to understand behavior.  
Or also on reflection, functions that are called depending on naming or behavior depending on naming.  

Add documentation to make clear what each part responsible for. Use clear variable names.  
Before every function always add a human-readable comment describing the function, even if it is redundant. Anyone should be able to recreate the function from the comment.  

The comments should be formatted as if you where having a conversation explaining to a junior dev what a code does. Those are examples, feel free to vary on this:

```
    In this function we ...
    This class is responsible for ...
    This concerns ... from ...
    ...this is required because it is called from class ...
    This is a proxy ...
    This delegates ...
    This represents ...
    This is faster because ...
    This uses the data structure .... in this case because ...
    This function should ... and it does it by ..
    The way this function works is ... and it is needed because ... so we return ...
    Here we simply ... and return ...
    This class is responsible for ... this should not do x because this is the responsibility of class ...
    On this class we have all ... because having them together makes it easier to find an maintain ...
    Here we isolate the logic for ... so we don't have it on ...
    This function could be joined with ... but I decided not to do it because ... to maintain ...
    This is here to isolate code responsible for ...
    The way this works is ...
    ... and pay attention because the behavior here is ...
    ... is unusual because we need to avoid ....
```

Always add logs, always add a trace log at the start of a function.
Add only few info logs, only for important events or big logic branching.

For all else, follow the decision chain: For who am I writing the log?

    If for Devs: Do I need t log variables? If yes is a debug, if not it is a trace If writing logs to system operators:
    Do I log because of unwanted or unexected state? If no, it is info
    Can the process continue with the unwanted or unexected state? If yes, use warn
    Can the application continue with the unwanted or unexected state? If yes, use error, if not use fatal

About the levels:

    info: Use sparsingly, only for inportant events
    warn: Explain why the state is unwanted or unexected
    error: Explain why the state is unwanted or unexected and consequences
    trace: Use a lot, basically as frequent as comments, should contain the funcion, format as a narrative such as, "foo(int a, float b): now this happens with a" or "bar(): after we have value y, we do xyz to get the zyx" and so on...
    debug: Use a lot, format as a sentence such as, "Sending int x to database" or "Mapping x to y" and so on...

Basically, info should inform me the ap is running ok, trace should give me a narraive that describe the whole flow of data and debug should besides all that give me values so I can figure out why things happens with the given values.

Use commmon log engine, configure it so it knows the class the looger is being called from and it is easy to filter.

Always add tests. Add tests for the happy path and for errors. Always ask your self, what could go wrong here, then add a test to ensure the app has a good behavior.  

Move important code to the left and keep boiler plate without line breaks. For Example, here the important thing is the name and age set: Instead of

```
foo
  .newBuilder()
  .unwrap()
  .value()
  .setName("foobar")
  .setAge(50)
  .wrap()
  .build();
```

prefer

```
foo.newBuilder().unwrap().value()
  .setName("foobar")
  .setAge(50)
.wrap().build();
```
See that tabulation wraps the actual important part. Someone reading can ignore the boilerplate, code is equivalent to this

```
foo {
  .setName("foobar")
  .setAge(50)
};
```
Try to keep the code width small. It is easier to read code if you don't keep moving your eye to the right, but top to bottom.  

Try to have as much compile time ensurance as possible. That means static typing, unit tests, microtypes with verified values (like ValidName instead of String, that you can only instantiate if the name is valid).  
Verify as early as possible so further code dont need to recheck validity.  
example:
Instead of
```
function x(string url){
  if(isValid(url)){
    do something;
  }
}
```
prefer
```
function x(URL url){ // Where Url can only be created if url string is valid
  do something;
}
```

Good code also needs refactoring, if you find that some code defines an important business concept, it may need their own folder and some refatorings. Or if code is changed always together, maybe they need to live together in a new folder.  
You can find out those with smart git commands. Every time you feel this is happening, use git to confirm.  
Try using automated refactoring if available such as:  
Rename function/variable  
Extract function  
Extract to new variable  
Move to new file  
Inline  
Others...  

# Documentation  

Keep two separate folders for documentation. Docs_technical and Docs_user. On technical be very technical, write freely about business logic, decisions gotchas, warnings, sequence diagrams, technical decisions and so on. This is for you and technical people.  
On user, try to always keep the user perspective, do not go into implementation or internal details, focus on whatever interface the user uses for interacting with the software (ui or cli or wathever). Do not mention code, or programming language or anything unless necessary or if it is expected the user know this. Keep the user docs direct, have a glossary and try to apply the Descartes method, where you don`t bring terms if they were not explained before (use links). If a term needs definition, a section, or a a chapter needs clarification, depends on the audience (the dev for technical docs and the user level for user docs) 

