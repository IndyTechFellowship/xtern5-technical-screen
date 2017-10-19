# General Rules


### Platform

- Submissions for the Software Engineering screen will be uploaded to Interviewzen
- You DO NOT need to write your code within the Interviewzen editor
- You MAY write your code in your own ide / editor and copy paste
- Once the screen is opened in Interviewzen you must submit your solution for a problem before being taken to the next
 - It is suggested that you review the questions here, and then go to the Interviewzen when you are prepared to submit your solutions
- Within Interviewzen, you MUST use use the same email that you are using on this Technical Screen form


### Languages

- You may use any mainstream language you desire. We will define the term "mainstream" as any language that appears in the top 50 languages on the [TIOBE index](https://www.tiobe.com/tiobe-index/). 
- You may mix-and-match languages between sections.
- You may **not** use any non-standard libraries in the language you choose.
- Some problems outline limitations which select parts of some languages' standard libraries that we believe make the problems _too_ easy. Using these parts of the standard library will cause you to lose points. 

### IO

- Program input is always provided through standard in as a stream of events. In other words, we should be able to call your program like `cat testdata | ./however_you_run_your_program`. 
- Program output should always be written in standard out, usually through something like `print` or `console.log`. 
- The format of the input will always be streamable. However, the problem definitions may define limitations which force you to wait for all the input before streaming the output (for example: requiring ordering of output).
- Nothing beside the problem solution should be written to stdout.

### Errors

- Some problems may define input assumptions which should cause your program to fail normal operation.
- If this happens, you should terminate and write an appropriate error to standard error, usually through something like `console.error`. 

### Bonus Points

- These prompts all contain assumptions you may make about the input.
- As long as your program conforms to what the assumptions require, you can choose to extend your program to support additional cases the assumptions don't cover.
- Doing so may earn your solution bonus points.
- There are also additional ways to earn bonus points we don't outline. For example; if the problem requires storing data, try writing additional (uncalled and non-printing) functions that retrieve or display that data. Just because it doesn't execute doesn't mean we won't see it.

### Issues/Concerns

- Have a question or concern? Open an Issue right here in Github and we'll get back to you ASAP!

# OSXtern

To make the lives of their Xterns easier, TechPoint has decided to commission the creation of a computer operating system, built by Xterns, for Xterns. Of course, you're on the team to help build it. In fact, you're on multiple teams. Let's tackle some of the problems this OS needs solved.

# (1) User-Group Reporting

Every modern operating system has a user accounts system, with the ability to add users to groups to control things like file access. We need to write a component of the operating system which can create these groups as administrators give users access, then print the groups back to the administrator.

_Given requests to add users to a group, output a list of the members of each group._

### Input Assumptions

- Input is provided as a newline-separated string through stdin.
- The first space-separated word on each line is the user's username. This username will always match the regex `[A-Za-z ]+`.
- The second space-separated word on each line is the group the user needs to be added to. This group will always match the regex `[A-Za-z ]+`.
- There is no guarantee of the ordering of this input.

### Input Example

```
alan admins
beth students
charlie teachers
david admins
```

### Output Assumptions

- Output should be provided through stdout in a comma-separated format, similar to CSV and described below.
- Each line takes the form `group,user1,user2,user3...`, until all the users in that group are listed. 
- Ensure that the output lines are ordered alphabetically by group name. 
- Ensure the users list on each line are ordered alphabetically within the line.
- Do not print empty "columns" in the output. For example, `admins,mike,david,,,` is invalid output.

### Output Example

```
admins,alan,david
students,beth
teachers,charlie
```

### Tips

- Remember that you cannot use any non-standard libraries in your implementation. Specifically, most languages don't have csv writing logic provided in the standard library.
- But also remember that our output format isn't perfect CSV; we don't print a header row, and we don't print empty columns. 
- You don't have to worry about escaping commas; per the regexes provided above, nothing in the input will contain commas.

# (2) NetXtern

Our operating system will be shipping with an internet browser. One of the core components of an internet browser is the ability to parse links on buttons and `<a>` tags, as well as maintain a history list with back/forward behavior. Today, you're going to implement that behavior.

_Given a stream of href links, backward operations, or forward operations, output a stream of the full url the browser should visit after each event in the input stream._

### Background: Href Links

First, we will provide you with some background on href links in browsers. There are three valid forms of href links that may appear in the input.

1. Absolute URLs, like `https://google.com`. These are identified by the URL scheme in the front, which you may assume is always `https://`.
2. Absolute Paths, like `/search`. These are identified by a leading forward-slash (`/`).
3. Relative Paths, like `query`. These are identified through the virtue of being neither of the former types of links.

### Input Assumptions

- Input is provided as a newline-separated string through stdin.
- Each line will be one of three things: the string `BACK` representing a back button click, the string `FORWARD` representing a forward button click, or an href link as described above.
- The stream is contiguous; each operation is provided in-order and acts on browser's currently visited page based on the previous operation.
- You may assume that Absolute URLs always follow the regex `https:\/\/[A-Za-z0-9]+\.com`.
- Absolute Paths and Relative Paths provided may contain more than one path component, such as `/r/programming` (the path components here include `r` and `programming`, separated by forward slashes). 
- You may assume that path components in Absolute and Relative paths follow the regex `[A-Za-z0-9]+`. More specifically, this means you do not have to handle the interesting security concern of properly handling `..` characters in paths.
- You may assume that `BACK` and `FORWARD` operations will always occur at times when they make sense. For example, a `BACK` operation makes no sense as the first operation in the stream, and a `FORWARD` operation makes no sense without a previously occurring `BACK` operation.

### Input Example

```
https://google.com
/search
query
term/kittens
/calendar/today
events/mine
https://reddit.com
r/kittens
BACK
FORWARD
new
```

### Behavior Example

Let's pretend we're clicking those links in order, and walk through what we expect to happen.

| Step | Operation            | Link Type     | Set Browser Context To                   |
| ---- | -------------------- | ------------- | ---------------------------------------- |
| 0    | `https://google.com` | Absolute URL  | `https://google.com`                     |
| 1    | `/search`            | Absolute Path | `https://google.com/search`              |
| 2    | `query`              | Relative Path | `https://google.com/search/query`        |
| 3    | `term/kittens`       | Relative Path | `https://google.com/search/query/term/kittens` |
| 4    | `/calendar/today`    | Absolute Path | `https://google.com/calendar/today`      |
| 5    | `events/mine`        | Relative Path | `https://google.com/calendar/today/events/mine` |
| 6    | `https://reddit.com` | Absolute URL  | `https://reddit.com`                     |
| 7    | `r/kittens`          | Relative Path | `https://reddit.com/r/kittens`           |
| 8    | `BACK`               |               | `https://reddit.com`                     |
| 9    | `FORWARD`            |               | `https://reddit.com/r/kittens`           |
| 10   | `new`                | Relative Path | `https://reddit.com/r/kittens/new`       |


And so on!

### Output Assumptions

- Your program should log the content of the "Set Browser Context To" field outlined above; the final full URL the browser should visit after every operation.
- Your output should be newline terminated after each operation.
- Each line of output should never end with a trailing `/`. 

### Output Example

```
https://google.com
https://google.com/search
https://google.com/search/query
https://google.com/search/query/term/kittens
https://google.com/calendar/today
https://google.com/calendar/today/events/mine
https://reddit.com
https://reddit.com/r/kittens
https://reddit.com
https://reddit.com/r/kittens
https://reddit.com/r/kittens/new
```

### Limitations

- Many standard libraries include functionality to do this url parsing and following for you. In some very popular langauges, a partial solution to this problem could be a only two or three lines. **Stay away from any url or http functionality in your standard libraries**. You might earn a few points for doing it the way you should totally do this on the job, but we're interested in how you think through the logic and the data structures you use to make this happen. 
- That being said, if there's a way to do it with a standard library function, feel free to talk it out in the comments alongside your actual solution. You may earn bonus points for identifying that functionality.

### Tips

- What you're being asked to build here essentially conforms to a sub-set of the functionality defined in IETF RFC 3986 Section 5 - "Reference Resolution".
- The assumptions outlined above significantly limit the scope of what you're required to build to be simpler, but anything above-and-beyond and still outlined in that RFC will be eligible for bonus points.
- Take care: the strings `BACK` and `FORWARD` are technically valid relative paths. Your solution needs to ensure that they are not treated as such, and instead are treated as special operations with different logic.

# (3) Activity Monitor

Ctrl-Alt-Delete; one of Windows' best gifts to the word. OSXtern needs similar functionality; the ability to track which processes are currently running on the system.

When a process is launched in OSXtern, it is given a unique process id and the launch is recorded in an event log. When the process exits, an event is also recorded. After closing, the process id may be reused by OSXtern for future processes.

Unfortunately, the person who wrote the event log logic didn't think about including whether an event is a "process start" or "process finished" event. 

_Given a stream of process ids associated with process start and finish actions, output the one process id which has not yet finished, or 0 if all processes are accounted for._

### Input Assumptions

- Input is provided as a new-line separated list of process IDs. 
- Each process ID is a positive integer between `1` inclusive and `2^16` exclusive.
- Each line in this log is untagged; it contains no information to determine whether an event is a process start or a process finish.
- Process IDs may be reused by the operating system after the previous process has finished execution. 

### Input 1 Example

```
36
47
58
47
36
```

### Input 1 Explanation

| Step | Process ID | What Happened? |
| ---- | ---------- | -------------- |
| 0    | 36         | Start          |
| 1    | 47         | Start          |
| 2    | 58         | Start          |
| 3    | 47         | Finish         |
| 4    | 36         | Finish         |

After parsing this input, we can determine that process `58` is still running.

### Output 1

```
58
```

### Input 2 Example

```
12
23
34
34
12
23
```

After parsing this input, we can conclude that all processes have finished execution.

### Output 2

```
0
```

### Tips

- You may assume that there will never be more than one process still running on your system. 
- Now, what you're thinking is "wouldn't this utility I'm writing also be a process, so there's no way my computer would only have one process running." You're not wrong, but also remember that OSXtern isn't real, so it isn't bound by the traditional laws of UNIX, like more mainstream plebian operating systems.
