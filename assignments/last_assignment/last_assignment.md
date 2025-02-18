# SI 506 The last assignment

## 1.0 Overview

| Reminders | Description |
| :---- | :------ |
| Available | Tuesday, 23 April 2024, 4:00 PM Eastern |
| Due | On or before Thursday, 02 May 2024, 11:59 PM Eastern |
| Points | Challenges 01-15: 1000 points |
| Authorship | Submitted solution must constitute your own work. Seeking and/or securing the assistance of others is prohibited; assisting other classmates who attempt the assignment is prohibited. |
| Web resource access | Permitted. Open network, open readings, open notes. You may refer to code in previous lecture exercises, lab exercises, and problem sets for inspiration. |
| Generative AI tool use | [UMGPT](https://umgpt.umich.edu/) and VS Code's [GitHub Copilot extension](https://code.visualstudio.com/blogs/2023/03/30/vscode-copilot) permitted. No other generative AI tools can be used to assist in the completion of this assignment. |
| Questions/Help | Limited to scheduled office hours and asynchronous interactions with the teaching team over the Slack SI 506 workspace `# last_assignment` channel. Format messages per the specified rules. Do not send direct messages (DMs) to individual teaching team members unless a personal issue arises that blocks your progress. If install, configuration, or hardware issues arises post a message to the `# install` channel. If a personal issue arises during the assignment period please send a private DM to Anthony immediately. |
| Code styling | Format code using the [VS Code Black Formatter](https://marketplace.visualstudio.com/items?itemName=ms-python.black-formatter) extension. Change `line-length` setting from 88 characters (default) to 100 characters. See VS Code [install guides](https://si506.org/guides/) for instructions. |
| Debugging | Ensure you run your code locally and review any runtime exceptions. Use the VS Code debugger, the built-in `print()` function, and/or `assert` statements to check your code. For configuring the debugger see [VS Code debugger: launch.json settings](https://si506.org/guides/configure_vscode_debugger_launch_json/).|
| Submission attempts | Unlimited between start and end dates/times. **Late submissions will not be accepted**. |
| Test failures | If you fail a test in Gradescope be sure to **read the error message** and any hints provided. If a traceback to an error is provided, review and resolve the runtime exception. Any tests involving student output files will automatically fail if a runtime exception is raised by your code.|
| Previous score activation | Permitted. If your final submission results in a score that is lower than a previous submission score you will be permitted to activate the earlier submission and claim the higher score. |
| Submission review | Submissions that do not earn 1000 points will be reviewed by the teaching team. Partial credit may be awarded for submissions that fail one or more autograder tests if the teaching team (at their sole discretion) deems a score adjustment is warranted. |
| Final course grade | Per the [Syllabus](https://si506.org/syllabus/), section 5.4, you _must_ meet the following two (`2`) requirements in order to earn a final course grade of `A`: earn a minimum `4650` course points _and_ **attempt all challenges comprising the last assignment**. |

## 2.0 Theme, files, imports, and caching

The Star Wars saga has spawned films, animated series, books, music, artwork, toys, games, fandom
websites, emojis, cosplayers, scientific names for new organisms (e.g.,
[_Trigonopterus yoda_](https://en.wikipedia.org/wiki/Trigonopterus_yoda)), and even a Darth Vader
_grotesque_ attached to the
[northwest tower](https://en.wikipedia.org/wiki/Darth_Vader_grotesque#/media/File:Darth_vader_grotesque.jpg)
of the Washington National Cathedral. Leading US news sources such as the
[New York Times](https://www.nytimes.com/) cover the Star Wars phenomenon on a regular basis.

The last assignment adds yet another Star Wars-inspired artifact to the list. The data used in this
assignment is sourced from the [Star Wars API](https://swapi.py4e.com/) (SWAPI),
[Wookieepedia](https://starwars.fandom.com/wiki/Main_Page),
[Wikipedia](https://www.wikipedia.org/), and the
[New York Times](https://www.nytimes.com/spotlight/star-wars).

### 2.1 Files

In line with the weekly lab exercises and problem sets you will be provided with a number of
files:

| File | Purpose |
| :-- | :------ |
| `last_assignment.md` | Assignment instructions |
| `last_assignment.py` | Program/script including a `main()` function and other definitions and statements. |
| `five_oh_six_utils.py` | Module containing utility functions and statements. |
| `data-clone_wars_episodes.csv` | Data file. |
| `data-jedi.json` | Data file. |
| `data-key_mappings.json` | Data file. |
| `data-nyt_star_wars_articles.json` | Data file. |
| `data-wookieepedia_droids.json` | Data file. |
| `data-wookieepedia_people.json` | Data file. |
| `data-wookieepedia_planets.csv` | Data file. |
| `data-wookieepedia_starships.csv` | Data file. |
| `fxt_*.json` | Collection of test fixture files that you must match with the files you produce. |

Please download the assignment files from Canvas Files as soon as they are released. This is a timed
event and delays in acquiring the assignment files will shorten the time available to engage with
the challenges. The clock is not your friend.

:exclamation: _DO NOT_ modify or remove the scaffolded code that we provide in the Python script or
module files unless instructed to do so.

### 2.2 Module imports

The template file `last_assignment.py` includes the following `import` statements:

```python
import copy
import five_oh_six_utils as utl

from pathlib import Path
```

The utilities module `five_oh_six_utils.py` includes the following `import` statements:

```python
import csv
import json
import requests

from urllib.parse import quote, urlencode, urljoin
```

:exclamation: **Do not** comment out or remove these `import` statements. Check your `import`
statements periodically. If you discover that other `import` statements have been added to
your Python files remove them. In such cases, VS Code is attempting to assist you by inserting
additional `import` statements based on your keystrokes. Their presence can trigger
`ModuleNotFoundError` exceptions when you submit your code to Gradescope.

### 2.3 Caching

As discussed in class, this assignment utilizes a caching workflow that eliminates
redundant HTTP GET requests made to SWAPI by storing the SWAPI responses locally. Caching is
implemented _fully_ and all you need do is call the function `get_swapi_resource()` whenever you
need to retrieve a SWAPI representation of a person/droid, planet, species, or starship, either
locally from the cache or remotely from SWAPI. The cache dictionary is serialized as JSON and
written to `CACHE.json` every time you run `last_assignment.py`.

:exclamation: _Do not_ call the function named `utl.get_resource` directly. Doing so sidesteps the
cache and undercuts the caching optimization strategy.

## 3.0 Challenges

A long time ago in a galaxy far, far away, there occured the Clone Wars (22-19 BBY), a major
conflict that pitted the [Galatic Republic](https://starwars.fandom.com/wiki/Galactic_Republic)
against the breakaway
[Separatist Alliance](https://starwars.fandom.com/wiki/Confederacy_of_Independent_Systems). The
Republic fielded genetically modified human clone troopers commanded by members of the Jedi order
against Separatist battle droids. The struggle was waged across the galaxy and, in time, inspired an
animated television series entitled
[_Star Wars: The Clone Wars_](https://starwars.fandom.com/wiki/Star_Wars:_The_Clone_Wars) which
debuted in October 2008 and ran for seven seasons (2008-2014, 2020).

The last assignment features four groups of challenges:

**Challenge 01_03**. Implement a number of `utl.to_*()` functions employing `try` and `except`
blocks that will be employed in later challenges.

:bulb: Any challenges that implement `utl.*` function definitions will be completed in the provided
`"five_oh_six_utils.py"` module. Any challenges/task referencing `main()` or functions without the `utl`
alias will be completed in the provided `"last_assignment.py"` file.

**Challenges 03-05**. Reimplement the function `read_csv_to_dicts()`. Utilize a _Clone Wars_ data
set that provides summary data about the animated series. Implement a number of functions
that will simplify interacting with the data in order to surface basic information about the episodes
and their directors, writers, and viewership.

**Challenges 06-07**. Work with New York Times article data that charts the creative, cultural, and
economic impact of the _Star Wars_ saga both within the US and elsewhere over the past forty-six
years.

**Challenges 08-15**. Recreates the escape of the Corellian G9 Rigger-class light freighter
[_Twilight_](https://starwars.fandom.com/wiki/Twilight) from the sabotaged and doomed Separatist
heavy cruiser [_Malevolence_](https://starwars.fandom.com/wiki/Malevolence) which took place
during the first year of the _Clone Wars_ conflict (22 BBY).

Your task is to reassemble the crew of the _Twilight_ and take on passengers before disengaging from
the _Malevolence_ and heading into deep space. The Jedi generals
[Anakin Skywalker](https://starwars.fandom.com/wiki/Anakin_Skywalker) and
[Obi-Wan Kenobi](https://starwars.fandom.com/wiki/Obi-Wan_Kenobi) together with the astromech droid
[R2-D2](https://starwars.fandom.com/wiki/R2-D2) had earlier boarded the _Malevolence_
after maneuvering the much smaller _Twilight_ up against the heavy cruiser and
docking via an emergency air lock. Their mission was twofold:

   1. Retrieve the Republican Senator
      [Padmé Amidala](https://starwars.fandom.com/wiki/Padm%C3%A9_Amidala) and the protocol
      (communications) droid [C-3PO](https://starwars.fandom.com/wiki/C-3PO) whose ship had been
      seized after being caught in the _Malevolence's_ tractor beam.
   2. Sabotage the warship.

In these challenges you will implement functions and follow a workflow that generates a JSON
document that recreates the _Twilight's_ escape from the _Malevolence_.

**May the Force be with You.**

## 3.1 Challenge 01 (60 points)

**Task**: Implement the functions `utl.to_float()` and `utl.to_int()`. Each function attempts to
convert a passed in `value` to a more appropriate type.

### 3.1.1 Implement `utl.to_float()`

Replace `pass` with a code block that attempts to convert the passed in `value` to a `float`.
Review the function's docstring to better understand the task it is to perform, the parameters it
defines, and the return value it computes.

### Requirements

1. Employ `try` and `except` statements in order to handle runtime exceptions whenever an invalid
   conversion is attempted.

   :exclamation: **Do not** place code outside the `try` and `except` code blocks.

2. The function _must_ convert numbers masquerading as strings, including those with commas that
   represent a thousand separator (e.g., "500,000,000", "5,000.75") to a floating point number.

3. Make use of the built-in function
   [`isinstance()`](https://www.w3schools.com/python/ref_func_isinstance.asp) to confirm that the
   passed in `value` is a string. Some strings may need to be manipulated before attempting the
   float conversion.

   :bulb: `isinstance()` accepts two arguments: an object (e.g., a string, list, etc.) and a
   type (e.g., `str`). The function returns `True` if the object is of the specified type;
   otherwise, it returns `False`.

4. If a runtime exception is encountered "catch" the exception in the `except` block and return the
   `value` to the caller **unchanged**.

   :bulb: You do not need to specify specific exceptions in the `except` statement.

### 3.1.2 Test `utl.to_float()`

After implementing the function return to `main()`. Uncomment the relevant `assert` statements.
Run your code. If an `AssertionError` is raised, debug your code, and then retest. Repeat as
necessary.

### 3.1.3 Implement `utl.to_int()`

Replace `pass` with a code block that attempts to convert the passed in `value` to an `int`. Review
the function's docstring to better understand the task it is to perform, the parameters it defines,
and the return value it computes.

### Requirements

1. Employ `try` and `except` statements in order to handle runtime exceptions whenever an invalid
   conversion is attempted.

   :exclamation: **Do not** place code outside the `try` and `except` code blocks.

2. The function _must_ convert numbers masquerading as strings, including those with commas that
   represent a thousand separator (e.g., '500,000,000') _and_ those with a period that designates a
   fractional component (e.g., '500,000,000.9999'), to a whole number.

   :bulb: Given the possibility that a number masquerading as a string may contain a fractional
   component that commences with a period (`.`) consider leveraging `to_float()` in your expression.

3. If a runtime exception is encountered "catch" the exception in the `except` block and return the
   `value` to the caller **unchanged**.

   :bulb: You do not need to specify specific exceptions in the `except` statement.

### 3.1.4 Test `utl.to_int()`

After implementing the function return to `main()`. Uncomment the relevant `assert` statements.
Run your code. If an `AssertionError` is raised, debug your code, and then retest. Repeat as
necessary.

## 3.2 Challenge 02 (60 points)

**Task**: Implement the functions `utl.to_list()`and `utl.to_none()`. Each function attempts to
convert a passed in `value` to a more appropriate type.

### 3.2.1 Implement `utl.to_list()`

Replace `pass` with a code block that attempts to convert the passed in `value` to a `list` using a
`delimiter` if one is provided. Review the function's docstring to better understand the task it is
to perform, the parameters it defines, and the return value it computes.

### Requirements

1. Employ `try` and `except` statements in order to handle runtime exceptions whenever an invalid
   conversion is attempted.

   :exclamation: **Do not** place code outside the `try` and `except` code blocks.

2. If the caller provides a `delimiter` value the function _must_ use it to split the `value`;
   otherwise, split the string without specifying a delimiter value.

   :bulb: Let the truth value of `delimiter` determine how you choose to split the string.

   :exclamation: Don't assume that `value` is "clean"; program defensively and remove
   leading/trailing spaces before attempting to convert the string to a list.

3. If a runtime exception is encountered "catch" the exception in the `except` block and return the
   `value` to the caller **unchanged**.

   :bulb: You do not need to specify specific exceptions in the `except` statement.

### 3.2.2 Test `utl.to_list()`

After implementing the function return to `main()`. Uncomment the relevant `assert` statements.
Run your code. If an `AssertionError` is raised, debug your code, and then retest. Repeat as
necessary.

### 3.2.3 Implement `utl.to_none()`

Replace `pass` with a code block that attempts to convert the passed in `value` to `None`. Review
the function's docstring to better understand the task it is to perform, the parameters it defines,
and the return value it computes.

### Requirements

1. Employ `try` and `except` statements in order to handle runtime exceptions whenever an invalid
   conversion is attempted.

   :exclamation: **Do not** place code outside the `try` and `except` code blocks.

2. In the `try` block check if the passed in `value` can be found in the passed in `none_values`
   list. If a match is obtained return `None` to the caller; otherwise, return the `value` unchanged.

   :exclamation: Don't assume that `value` is "clean"; program defensively and remove
   leading/trailing spaces before checking if the "cleaned" version of the string matches a
   `none_values` item.

3. If a runtime exception is encountered "catch" the exception in the `except` block and return the
   `value` to the caller **unchanged**.

   :bulb: You do not need to specify specific exceptions in the `except` statement.

### 3.2.4 Test `utl.to_none()`

After implementing the function return to `main()`. Uncomment the relevant `assert` statements.
Run your code. If an `AssertionError` is raised, debug your code, and then retest. Repeat as
necessary.

## 3.3 Challenge 03 (55 points)

**Task**: Refactor (e.g., modify) the function `utl.read_csv_to_dicts()` to use a
**list comprehension** and then call the function to read a CSV file that contains information about
the _Clone Wars_ episodes. Then implement the function `has_viewer_data()` that checks whether or
not an episode possesses viewership information.

:bulb: This challenge involves a list of nested dictionaries. Use the built-in function `print()` to
explore one of the nested dictionaries or call the function `utl.write_json()` in `main()`, encode the
data as JSON, and write it to a "test" JSON file so that you can view the list of dictionaries more
easily.

### 3.3.1 Refactor `utl.read_csv_to_dicts()`

Examine the commented out code in `utl.read_csv_to_dicts()` function (**do not** uncomment).
Reimplement the function by writing a **list comprehension** that performs the same actions as the
commented-out code. Then return the new list created by the comprehension to the caller.

:exclamation: If you are unsuccessful in your endeavors to implement a list comprehension uncomment
the code in `utl.read_csv_to_dicts()` and get the function working so that you can continue with the
assignment.

### Requirements

1. You are limited to writing one (`1`) line of code by employing a list comprehension.

2. You _must_ employ **existing variable names** that appear in the commented out code when
   writing your list comprehension.

   :exclamation: Utilize only those variable names that are relevant to the task at hand.

### 3.3.2 Test `utl.read_csv_to_dicts()`

After refactoring `utl.read_csv_to_dicts()` return to `main()`.

1. Call the function and retrieve the data contained in the file `data-clone_wars_episodes.csv`.

2. Assign the return value to a variable named `clone_wars_episodes`.

### 3.3.3 Implement `has_viewer_data()`

Replace `pass` with a code block that checks whether or not an individual _Clone Wars_ episode
possesses viewership information. Review the function's docstring to better understand the task it
is to perform, the parameters it defines, and the return value it computes.

### Requirements

1. The function _must_ compute the truth value of the passed in episode's "episode_us_viewers_mm"
   key-value pair, returning either `True` or `False` to the caller.

   :bulb: Recall that a function can include more than one `return` statement. That said, you can
   also employ Python's ternary operator to solve this challenge with a single line of code.

### 3.3.4 Call `has_viewer_data()`

After implementing the function return to `main()`.

1. Test your implementation of `has_viewer_data()` by counting the number of episodes in the
   `clone_wars_episodes` list that possess a "episode_us_viewers_mm" numeric value. Employ a loop,
   a conditional statement, and the accumulator pattern to accomplish the task. Whenever the return
   value of `has_viewer_data()` equals `True` increment your episode count by 1.

   :bulb: Recall that a function call is considered an expression and `if` statements are composed
   of one or more expressions.

2. Uncomment the relevant `assert` statement. Run your code. If an `AssertionError` is raised, debug
   your code, and then retest. Recheck both your implementation of `has_viewer_data()`, your `for`
   loop and loop block `if` statement. Repeat as necessary.

### 3.2.4 Test `utl.to_none()`

After implementing the function return to `main()`. Uncomment the relevant `assert` statements.
Run your code. If an `AssertionError` is raised, debug your code, and then retest. Repeat as
necessary.

## 3.4 Challenge 04 (50 points)

**Task**: Implement a function that converts _Clone Wars_ episode string values to more appropriate
types.

### 3.4.1 Implement `convert_episode_values()`

Replace `pass` with a code block that converts specifed string values to more appropriate types.
Review the function's docstring to better understand the task it is to perform, the parameters it
defines, and the return value it computes.

### Requirements

1. The function accepts a list of nested "episode" dictionaries. You _must_ implement a nested
   loop to perform the value type conversions.

   * Outer loop: passed in `episodes` list of nested dictionaries.
   * Inner loop: individual "episode" dictionary items.

2. Employ `if-elif-else` conditional statements to convert the values encountered to more
   appropriate types. Utilize the conditional statements to perform the following operations on each
   key and value encountered:

   1. Check if the value is a member of `none_values`. If `True` assign `None` to the converted
      value's associated key.

   2. Convert certain episode string values to more appropriate types by passing the value to the
      appropriate `utl.to_*()` function and assigning the return value to the converted value's
      associated key. See the docstring's "Type conversions" section for a listing of required
      conversions.

      :bulb: Consider which conversions might be grouped together in your conditions. Remember that
      there can be more than one `elif` condition if needed.

3. After the outer loop terminates return the list of mutated dictionaries to the caller.

### 3.4.2 Call `convert_episode_values()`

After implementing the function, return to `main()`.

1. Call the function `convert_episode_values()` and pass it the following argument:
   `clone_wars_episodes` and the constant `NONE_VALUES`. Assign the return value to
   `clone_wars_episodes`.

2. Call the function `utl.write_json()` and write `clone_wars_episodes` to the file
   `stu-clone_wars-episodes_converted.json`. Compare your file to the test fixture file
   `fxt-clone_wars-episodes_converted.json`. Both files _must_ match, line-for-line, and
   character-for-character.

## 3.5 Challenge 05 (50 points)

**Task**: Implement a function that retrieves the most viewed _Clone Wars_ episode(s).

### 3.5.1 Implement `get_most_viewed_episode()`

Replace `pass` with a code block that finds the most viewed _Clone Wars_ episode(s) in the data
set. Review the function's docstring to better understand the task it is to perform, the parameters
it defines, and the return value it computes.

### Requirements

1. The function _must_ return a list of one or more episodes from the passed in
   `episodes` list that feature the highest recorded viewership. Include in the list
   only those episodes that tie for the highest recorded viewership. If no ties exist
   only one episode will be returned in the list.

2. The function _must_ exclude from the list all episodes that lack a viewership value.

3. Delegate to `has_viewer_data()` the task of checking whether an episode contains a _truthy_
   "episode_us_viewers_mm" value. The function needs to check the truth value of
   "episode_us_viewers_mm" before you attempt to compare the current "episode_us_viewers_mm" value
   to the previous value.

   :bulb: Assign two local "accumulator" variables to the viewer count and the top episode(s).

### 3.5.2 Call `get_most_viewed_episode()`

After implementing the function return to `main()`.

1. Call the function and pass it the following argument: `clone_wars_episodes`.

2. Assign the return value to `most_viewed_episode`. Pass the variable to `print()`
   and review the terminal output.  If the list contains the following elements
   proceed to the next challenge; if not, recheck your code.

   ```python
   [
      {
         'series_title': 'Star Wars: The Clone Wars',
         'series_season_num': 1,
         'series_episode_num': 2,
         'season_episode_num': 2,
         'episode_title': 'Rising Malevolence',
         'episode_director': 'Dave Filoni',
         'episode_writers': ['Steven Melching'],
         'episode_release_date': 'October 3, 2008',
         'episode_prod_code': 1.07,
         'episode_us_viewers_mm': 4.92
         },
      {
         'series_title': 'Star Wars: The Clone Wars',
         'series_season_num': 7,
         'series_episode_num': 134,
         'season_episode_num': 13,
         'episode_title': 'The Lecturers',
         'episode_director': 'Anthony Whyte',
         'episode_writers': ['Anthony Whyte', 'Chris Teplovs'],
         'episode_release_date': 'May 7, 2020',
         'episode_prod_code': 7.25,
         'episode_us_viewers_mm': 4.92
         }
   ]
   ```

## 3.6 Challenge 06 (50 points)

**Task**: Implement the function `get_news_desks()`.

### 3.6.1 Implement `get_news_desks()`

Replace `pass` with a code block that returns a list of New York Times "news desks" sourced from the
passed in `articles` list. Review the function's docstring to better understand the task it is to
perform, the parameters it defines, and the return value it computes.

:bulb: Each article dictionary contains a "news_desk" key-value pair.

### Requirements

1. There are a number of articles with a "news_desk" value of "None" (a `str`). When looping over the
   articles, call the function `utl.to_none()` and pass it each article's "news_desk" value. Assign
   the return value to a local variable (e.g., `news_desk`).

   Check the truth value of this object before you evaluate whether or not the accumulator list
   named `news_desks` contains the value.

2. The list of news desk names returned by the function _must not_ contain any duplicate elements.
   Accumulate the values carefully.

3. The function _must_ return a new version of the accumulator list **sorted alphanumerically**.

### 3.6.2 Call `get_news_desks()`

After implementing the function return to `main()`.

1. Call the function `utl.read_json()` and retrieve the New York Times article data in the file
   `./data-nyt_star_wars_articles.json`. Assign the return value to `articles`.

2. Test your implementation of `get_news_desks()` by calling the function and passing to it the
   following arguments: `articles` and the constant `NONE_VALUES`. Assign the return value to the
   variable `news_desks`.

3. Call the function `utl.write_json()` and write `news_desks` to the file `stu-nyt_news_desks.json`.
   Compare your file to the test fixture file`fxt-nyt_news_desks.json`. The files _must_ match line
   for line, indent for indent, and character for character.

## 3.7 Challenge 07 (60 points)

**Task**: Implement the function `group_articles_by_news_desk()`.

### 3.7.1 Implement `group_articles_by_news_desk()`

Replace `pass` with a code block that returns a dictionary of "news desk" key-value pairs that
group the passed in `articles` by their parent news desk drawn from the `news_desks` list. Review
the function's docstring to better understand the task it is to perform, the parameters it defines,
and the return value it computes.

### Requirements

1. Assign an empty "accumulator" dictionary to a local variable.

2. Implement a nested loop. Review the `data-nyt_star_wars_articles.json` and `stu-nyt_news_desks.json`
   files and decide which list should be traversed by the outer loop and which list should be
   traversed by the inner loop.

   :bulb: The news desk name provides the link between the two lists.

3. Inside the nested loop create an "accumulator" dictionary key-value pair structured as follows:

   ```python
   some_accumulator_dict[< news_desk >] = []
   ```

   The list will hold "thinned" dictionary representations of the articles published by the news desk.

   :exclamation: There are two locations in the nested loop where this key-value pair can be placed:
   inside the outer loop or inside the inner loop. Choose wisely.

4. Each "thinned" article dictionary added to the "news_desk" list comprises a subset of the
   available key-value pairs ordered as follows:

   ```python
   {
      "web_url": "https://www.nytimes.com/2016/10/20/business/media/lucasfilm-sues-jedi-classes.html",
      "headline_main": "Classes for Jedis Run Afoul of the Lucasfilm Empire",
      "news_desk": "Business",
      "byline_original": "By Erin McCann",
      "document_type": "article",
      "material_type": "News",
      "abstract": "A man whose businesses offers private lessons and certifications for fine-tuning lightsaber skills is operating without the permission of the “Star Wars” owner.",
      "word_count": 865,
      "pub_date": "2016-10-19T13:26:21+0000"
   }
   ```

   :exclamation: Certain keys such as "headline_main", "byline_original", and "material_type" are
   not found in the original New York Times dictionaries. Hopefully, the names provide a sufficient
   hint about which values to map (i.e., assign) to each key.

### 3.7.2 Call `group_articles_by_news_desk()`

After implementing the function return to `main()`.

1. Call the function and pass it `news_desks` and `articles` as arguments. Assign the return value
   to the variable `news_desk_articles`.

2. Call the function `utl.write_json()` and write `news_desk_articles` to the file
   `stu-nyt_news_desk_articles.json`. Compare your file to the test fixture file
   `fxt-nyt_news_desk_articles.json`. The files _must_ match line for line, indent for indent, and
   character for character.

## 3.8 Challenge 08 (40 points)

**Task**: Implement the utility function `utl.get_nested_dict()`.

### 3.8.1 Implement `utl.get_nested_dict()`

Replace `pass` with a code block that utilizes a `filter` string to return a nested dictionary from
the passed in `data` list. Review the function's docstring to better understand the task it is to
perform, the parameters it defines, and the return value it computes.

The function is employed to traverse lists of nested dictionaries sourced from the
following files in search of a particular dictionary representation of a Star Wars droid,
person, planet, or starship:

* `data-wookieepedia_droids.json`
* `data-wookieepedia_people.json`
* `data-wookieepedia_planets.csv`
* `data-wookieepedia_starships.csv`

:bulb: Familiarize yourself with the Wookieepedia-sourced data files before commencing the remaining
challenges.

### Requirements

1. Loop over the nested dictionaries in the passed in `data` list.

2. Inside the loop block utilize the passed in `key` name to identify the key-value pair in the
   nested dictionary to evaluate. The value mapped to the `key` _must_ be compared to the passed in
   `filter` value using a **case sensitive** comparison. If an **exact match** is obtained the
   nested dictionary is returned to the caller; otherwise `None` is returned.

### 3.8.2 Call `utl.get_nested_dict()`

After implementing the function return to `main()`.

1. Call the function `utl.read_csv_to_dicts()` and retrieve the supplementary Wookieepedia planet
   data in the file `data-wookieepedia_planets.csv`. Assign the return value to `wookiee_planets`.

2. Call the function `utl.get_nested_dict()` and pass it the following arguments:
   `wookiee_planets`, the key "name", and the string "Dagobah".

3. Assign the return value to the variable `wookiee_dagobah`.

4. Call the function `utl.write_json()` and write `wookiee_dagobah` to the file
   `stu-wookiee_dagobah.json`. Compare your file to the test fixture file `fxt-wookiee_dagobah.json`
   The files _must_ match line for line, indent for indent, and character for character.

5. Call `utl.get_nested_dict()` a second time and pass it the following arguments:
   `wookiee_planets`, the key "system", and the string "Al'Har system".

6. Assign the return value to the variable `wookiee_haruun_kal`.

7. Call the function `utl.write_json()` and write `wookiee_haruun_kal` to the file
   `stu-wookiee_haruun_kal.json`. Compare your file to the test fixture file
   `fxt-wookiee_haruun_kal.json`. The files _must_ match line for line, indent for indent, and
   character for character.

## 3.9 Challenge 09 (35 points)

**Task**: Implement the function `utl.to_year_era()`.

### 3.9.1 Implement `utl.to_year_era()`

Replace `pass` with a code block that attempts to separate the Galactic standard calendar year and
era (e.g., 896BBY, 24ABY) value into a dictionary comprising year and era key-value pairs.

### Requirements

1. Employ `try` and `except` statements in order to handle runtime exceptions whenever an invalid
   conversion is attempted. **Do not** place code outside the `try` and `except` code blocks.

2. In the `try` block use slicing to access the year and era segments of the string.

   :bulb: Note that while the year segment's length varies (e.g., 896, 19, 0) the era segment of
   the Galactic calender date string comprises three characters: "BBY" or "ABY". Keep this in mind
   as you design your slicing expressions.

3. Before mapping the sliced segments to a dictionary, you _must_ first check if the "year" segment
   of the `value` is a number by employing the appropriate `str` method. If the substring
   **is numeric**&mdash;attempt to call the value's `isnumeric()` method&mdash;return a dictionary
   literal that maps the necessary slicing expressions to "year" and "era" keys as values. Structure
   the dictionary as follows:

   ```python
   {"year": < year > (int), "era": < era > (str)}
   ```

4. Otherwise, if the "year" segment is not considered numeric return the `value` to the caller
   **unchanged**.

5. If the year segment is numeric, convert the "year" segment to an integer by passing it as the
   argument to the function `to_int()`. Call the function from within the dictionary literal.

6. If a runtime exception is encountered "catch" the exception in the `except` block and return the
   `value` to the caller **unchanged**.

   :bulb: You do not need to specify specific exceptions in the `except` statement.

### 3.9.2 Test `utl.to_year_era()`

After implementing the function return to `main()`. Uncomment the relevant `assert` statements.
Run your code. If an `AssertionError` is raised, debug your code, and then retest. Repeat as
necessary.

## 3.10 Challenge 10 (40 points)

**Task**: Implement the function `transform_planet()`.

:bulb: For the next several challenges the workflow outlined below illustrates the general creational
pattern applied to each droid, person, planet, species, and starship encountered in the assignment.

<div style="background-color: #C1E1C1; border-radius: 10px; color: black; padding-top: 5px; padding-left: 3px; padding-right: 7px; padding-bottom: 3px">
   <ol>
      <li>
          Retrieve a SWAPI representation of the entity if one exists. Deserialize the JSON into a
          dictionary. Note that retrieving the entity via a querystring search requires accessing
          the entity from the decoded dictionary's "results" list.
      </li>
      <li>
          Retrieve a Wookieepedia representation of the entity if one exists. Read the data from
          a JSON or CSV file and assign the data to a local variable.
      <li>
          Perform an in-place operation that mutates the SWAPI dictionary with Wookieepedia
          data, if provided. The SWAPI entity serves as the default representation of the entities
          that feature in the assignment. The Wookieepedia data is used to enrich the SWAPI data
          with new and updated key-value pairs.
      </li>
      <li>
          Call the appropriate <code style="color: blue">transform_*()</code> function to return a
          a new dictionary comprising a specified subset of the original SWAPI/Wookieepedia key-value
          pairs, substituting in new keys when required and converting certain values to more
          appropriate types, including replacing certain string values with
          <code style="color: blue">None</code>. Use the appropriate nested dictionary read in from
          the "data-key_mappings.json" file to guide your transformations.
      </li>
      <li>
          Serialize the new dictionary as JSON and write the object to a file in order to review the
          output.
      </li>
   </ol>
</div>
<br />

### 3.10.1 Implement `transform_planet()`

Replace `pass` with a code block that returns a dictionary representation of a planet based on the
passed in `data` dictionary. Review the function's docstring to better understand the task it
is to perform, the parameters it defines, and the return value it computes.

### Requirements

1. Assign an empty dictionary to a local variable (name your choice). This "planet"
   dictionary will be employed to accumulate new key-value pairs sourced from the passed in `data`
   dictionary.

2. The function also requires that the `keys` dictionary be provided by the caller. The `keys`
   dictionary (obtained from the file "data-key_mappings.json") includes nested droid, person,
   planet, species, and starship dictionaries. Each of these nested dictionaries specify the
   following attributes of the new dictionary that each `transform_*()` function will construct:

   * the subset of `data` key-value pairs to be mapped to the new dictionary.

   * the order in which the `data` key-value pairs are mapped to the new dictionary.

   * the key names to be used in the new dictionary.

   For example, each key in the `keys` "planet" dictionary corresponds to a key in the passed in
   `data` dictionary. Each value in the `keys` "planet" dictionary represents the (new) key name to
   be used in the new planet dictionary.

   ```python
   keys = {
        ...
        "planet": {
            "url": "url", # old key: new key
            ...,
            "orbital_period": "orbital_period_days", # old key: new key
            ...
        },
        ...
    }
   ```

3. Loop over the `keys` dictionary's nested "planet" dictionary's keys or items (your choice).
   Inside the loop block employ `if-elif-else` conditional statements to populate the new planet
   dictionary with key-value pairs sourced from `data`.

   :bulb: Each key in the `keys` nested "planet" dictionary maps to a `data` key-value pair. The
   associated value provides the **new key** to utilize in the new dictionary.

4. Inside the loop convert `data` values to more appropriate types as outlined in the docstring's
   mappings section. Strings found in `none_values` must be converted to `None`
   **irrespective of case**. Delegate type conversions to the various `utl.to_*()` functions. Then
   map the `data` value to the new key when assigning the key-value pair to the new dictionary.

   For example, if a `keys` key is named "diameter" perform the following actions:

   1. access the corresponding `data` "diameter" value, converting it as required to a more
      appropriate type or `None` using the appropriate `utl.to_*()` functions.

   2. assign the (converted) value to the new dictionary using the `keys` nested "planet"
      dictionary's "diameter_km" value as the new key.

   :bulb: Review the mappings carefully. Opportunities exist to reduce the number of `elif`
   statements by grouping keys whose values are be converted to the same type.

### 3.10.2 Test `transform_planet()`

After implementing `transform_planet()` return to `main()`.

1. Create a `pathlib.Path` object that provides an **absolute** path to the file
   "data-key_mappings.json". Assign the object to a variable named `keys_path`.

2. Call the function `utl.read_json()` passing it the argument `keys_path`. Assign the return
   value to a variable named `keys`.

   :bulb: Review the nested JSON objects in the file `data-key_mappings.json`. Each object provides
   a mapping of _current_ SWAPI/Wookieepedia key names to _new_ key names
   (e.g., "length" -> "length_m") for each entity featuring in this assignment. After deserializing
   the JSON into a dictionary of dictionaries you will employ these mappings to create new
   dictionary representations of droids, people, planets, species, and starships that, in certain
   cases, feature new key names.

3. Call the function `transform_planet()` and pass it the arguments it requires to return a
   transformed dictionary representation of the planet Harunn Kal. Assign the variable `harunn_kal`
   to the return value of the function call.

4. Call the function `utl.write_json()` and write `harunn_kal` to the file `stu-haruun_kal.json`.
   Run your code. Compare your file to the test fixture file `fxt-haruun_kal.json`. Both files
   _must_ match line for line, indent for indent, and character for character.

## 3.11 Challenge 11 (105 points)

**Task**: Transform each planet in `wookiee_planets` and sort the new list using a `lambda`
function.

### 3.11.1 Transform `wookiee_planets`

1. Write a **single-line list comprehension** that transforms each Wookieepedia-sourced planet
   dictionary in the `wookiee_planets` list by calling the function `transform_planet()` inside the
   comprehension and passing to it the required arguments (including `keys`) in order to transform
   each planet encountered in the loop. Assign the new list to a variable named `planets`.

   :exclamation: If your list comprehension triggers a `KeyError` exception, check your
   implementation `transform_planet()`. The function is likely attempting to access a key in the
   planet dictionary that does not exist. Recall that there is a friendly dictionary method for
   dealing with such issues; refactor (i.e., revise) your function block accordingly.

### 3.11.2 Sort `wookiee_planets`

1. After returning a new list of transformed planet dictionaries, perform an **in_place** sort of
   the `planets` list passing to it as the `key` argument a `lambda` function that sorts the planets
   **by name**. **Reverse** the sort so that the planets are sorted by name in **descending order**.

2. Call the function `utl.write_json()` and write `planets` to the file
   `stu-planets_sorted_name.json`. Compare your file to the test fixture file
   `fxt-planets_sorted_name.json`. Both files _must_ match line for line, indent for indent, and
   character for character.

### 3.11.3 Sort planets by diameter and name

:bulb: If you get stuck on sorting `planets` by "diameter_km" and "name", pause your sorting work
and proceed to the next challenge. Then return and restart this standalone task.

1. Employ the built-in function `sorted()` and a `lambda` function to sort `planets` by the
   following attributes:

   | Key | Value | Order |
   | :-- | :---- | :---- |
   | diameter_km | `int` \| `None` | descending |
   | name | ascending | `str` |

2. You _must_ write your `lambda` expression using the **ternary operator** when sorting on
   "diameter_km" because several planets lack a known diameter and in consequence `None` has been
   mapped (i.e., assigned) to their "diameter_km" key.

3. Assign the return value of `sorted()` to a variable named `planets_diameter_km`.

4. Call the function `utl.write_json()` and write `planets_diameter_km` to the file
   `stu-planets_sorted_diameter.json`. Compare your file to the test fixture file
   `fxt-planets_sorted_diameter.json`. Both files _must_ match line for line, indent for indent, and
   character for character.

## 3.12 Challenge 12 (45 points)

> R2 are you quite certain that the ship is in this direction? This way looks potentially dangerous. _C-3PO_

**Task**: Implement the function `transform_starship()`.

### 3.12.1 Implement `transform_starship()`

Replace `pass` with a code block that returns a new starship dictionary based on the passed in
`data` dictionary. Review the function's docstring to better understand the task it is to perform,
the parameters it defines, and the return value it computes.

### Requirements

1. Assign an empty dictionary to a local variable (name your choice). This "starship" dictionary
   will be employed to accumulate new key-value pairs sourced from `data`.

2. Adopt the same key-value mapping pattern employed in the `transform_planet()` function. Loop over
   the `keys` "starship" dictionary (how best to do so is your choice). Inside the loop block employ
   `if-elif-else` conditional statements to populate the new starship dictionary with key-value pairs
   sourced from `data`, converting `data` values to more appropriate types as outlined in the
   docstring's mappings section.

### 3.12.2 Call `transform_starship()`

After implementing `transform_starship()` return to `main()`.

1. Call the `utl.read_csv_to_dicts()` function and retrieve the supplementary Wookieepedia starship
   data in the file `data-wookieepedia_starships.csv`. Assign the return value to `wookiee_starships`.

   :exclamation: The starship _Twilight_ is sourced from Wookieepedia **only**. No SWAPI
   representation of the Corellian G9 Rigger-class light freighter exists.

2. Call `utl.get_nested_dict()` and pass it the arguments required to retrieve the light
   freighter named [_Twilight_](https://starwars.fandom.com/wiki/Twilight) in `wookiee_starships`.
   Assign the return value to a variable named `wookiee_twilight`.

3. Call the function `transform_starship()` and pass it the arguments it requires to create a "thinned"
   representation of the starship. Assign the return value to a variable named `twilight`.

4. Call the function `utl.write_json()` and write `twilight` to the file `stu-twilight.json`. Run
   your code. Compare your file to the test fixture file `fxt-twilight.json`. Both files _must_
   match line for line, indent for indent, and character for character.

## 3.13 Challenge 13 (100 points)

> I'm gonna hotwire the ship and give Grievous a little surprise. _Anakin Skywalker_

**Task**: Implement the function `transform_sentient_being()`. The function handles creating new
dictionary representations of both organic (i.e., person) and mechanical (i.e., droid) sentient
beings.

:bulb: Note that a sentient being categorized as a _droid_ is considered "a mechanical being"
possessing both a "self-aware consciousness" and "a self-contained method of locomotion." (for more
info see, https://starwars.fandom.com/wiki/Droid/Legends).

### 3.13.1 Implement `transform_sentient_being()`

Replace `pass` with a code block that returns a new person or droid dictionary based on the passed
in `data` dictionary and `is_droid` value. Review the function's docstring to better understand the
task it is to perform, the parameters it defines, and the return value it computes.

### Requirements

1. Assign an empty dictionary to a local variable (name your choice). This "sentient_being"
   dictionary will be employed to accumulate new key-value pairs sourced from `data`.

2. Since this function is designed to return a dictionary representation of either a person (organic)
   or a droid (mechanical) special care _must_ be taken to access the correct nested dictionary in
   `keys`.

   Check the truth value of `is_droid` to determine the type of sentient being represented by the
   passed in `data` dictionary. This is required in order to select the appropriate "droid" or
   "person" nested dictionary in `keys`. Assign a variable (choose a generic name) to the nested
   dictionary retrieved from `keys`.

   :bulb: Consider using a single-line ternary operator to access the nested dictionary:

   ```python
   sentient_being_keys = < nested_dict_A > if < some_condition > else < nested_dict_B >
   ```

3. Adopt the same key-value mapping pattern employed in the other `transform_*()` functions. Loop
   over the `keys` nested droid or person dictionary's keys or itema. Inside the loop block employ
   `if-elif-else` conditional statements to populate the new droid or person dictionary with
   key-value pairs sourced from `data`, converting `data` values to more appropriate types as
   outlined in the docstring's mappings section.

   :exclamation: Review the person and droid old key/new key mappings in the docstring. Some overlap
   exists but certain keys are not shared between organic and mechanical sentient beings. For keys
   that are not shared consider using `is_droid` in certain conditional statements as an additional
   filtering expression. In such cases, the conditional statement that you implement will constitute
   a compound conditional.

4. Additionally, a person's "homeworld" and "species" key-value pairs require special handling
   (performed inside the loop).

   1. Delegate to the function `get_swapi_resource()` the task of retrieving a dictionary
      representation of the person's home planet.

   2. If the caller passes in a Wookieepedia-sourced `planets` list, delegate to the function
      `utl.get_nested_dict()` the task of retrieving the Wookieepedia representation of the
      "homeworld" from `planets`.

   3. If a Wookieepedia version of the planet is returned by calling `utl.get_nested_dict()`, mutate
      the SWAPI dictionary by enriching it with the Wookieepedia data. Employ an in-place operation
      to accomplish the task.

   4. Transforming the resulting SWAPI/Wookieepedia homeworld dictionary is delegated to the function
      `transform_planet()`.

   5. Likewise, delegate to the function `get_swapi_resource()` the task of retrieving a
      representation of the person's species. From the dictionary returned access the "name" value
      **only** and map (i.e., assign) the string to the new dictionary's "species" key.

### 3.13.2 Get Wookieepedia data

After implementing the function return to `main()`.

1. Call the function `utl.read_json()` and retrieve the supplementary Wookieepedia
   person data in the file `data-wookieepedia_people.json`. Assign the variable named `wookiee_people`
   to the return value of the function call.

2. Call the function `utl.read_json()` and retrieve the supplementary Wookieepedia
   droid data in the file `data-wookieepedia_droids.json`. Assign the variable named `wookiee_droids`
   to the return value of the function call.

### 3.13.3 Call transform_sentient_being()

1. Next, call the function `get_swapi_resource()` and retrieve a SWAPI representation of
   the Jedi knight [Anakin Skywalker](https://starwars.fandom.com/wiki/Anakin_Skywalker). Make use
   of the appropriate constant to simplify construction of the URL and pass the required search
   "params" value to retrieve Anakin. Access the "Anakin" dictionary which is stored in the response
   object and assign the value to `swapi_anakin`.

2. Call the function `utl.get_nested_dict()` and pass it the arguments required to
   retrieve the "Anakin Skywalker" dictionary in `wookiee_people`. Assign the return
   value to a variable named `wookiee_anakin`.

3. Combine the SWAPI and Wookieepedia person dictionaries. Mutate `swapi_anakin` by combining it
   with the Wookieepedia representation of the Jedi knight. Employ an in-place operation to
   accomplish the task.

4. Call the function `transform_sentient_being()` and pass it the arguments it requires to create a
   "thinned" representation of the Jedi knight. Assign the return value to a variable named `anakin`.

5. Call the function `utl.write_json()` and write `anakin` to the file
   `stu-anakin_skywalker.json`. Run your code. Compare your file to the test fixture file
   `fxt-anakin_skywalker.json`. Both files _must_ match line for line, indent for indent, and
   character for character.

### 3.13.4 Repeat the process (twice)

1. Create a "transformed" dictionary representation of Anakin Skywalker's astromech droid
   [R2-D2](https://starwars.fandom.com/wiki/R2-D2). Utilize the same creational workflow that
   combined Anakin's SWAPI and Wookiee dictionary representations. Use the following variable names
   as you transform the various dictionary representations of R2-D2: `wookiee_r2_d2`, `swapi_r2_d2`,
   and `r2_d2`.

2. Call the function `utl.write_json()` and write `r2_d2` to the file `stu-r2_d2.json`. Run your
   code. Compare your file to the test fixture file `fxt-r2_d2.json`. Both files _must_ match line
   for line, indent for indent, and character for character.

3. Finally, create a "transformed" dictionary representation of the
   [Jedi](https://en.wikipedia.org/wiki/Jedi) master and general
   [Obi-Wan Kenobi](https://starwars.fandom.com/wiki/Obi-Wan_Kenobi). Utilize the same workflow and
   variable name format (`wookiee_obi_wan`, `swapi_obi_wan`, and `obi_wan`) employed earlier to
   create `anakin` and `r2_d2`.

4. Call the function `utl.write_json()` and write `obi_wan` to the file
   `stu-obi_wan_kenobi.json`. Run your code. Compare your file to the test fixture file
   `fxt-obi_wan_kenobi.json`. Both files _must_ match line for line, indent for indent, and
   character for character.

## 3.14 Challenge 14 (135 points)

> Let's get back to the ship. Power up the engines R2. _Anakin Skywalker_

**Task**: Implement the function `board_ship()`. Assign Anakin Skywalker and Obi-Wan Kenobi to the
_Twilight_ as crew members. Get Senator Padmé Amidala, the protocol droid C-3PO, and the astromech
droid R2-D2 aboard the _Twilight_ as passengers.

### 3.14.1 Implement `board_ship()`

Replace `pass` with a code block that assigns to a ship crew members by crew postion and, optionally,
boards passengers. Review the function's docstring to better understand the task it is to perform,
the parameters it defines, and the return value it computes.

### Requirements

1. Crew members

   1. You _must_ employ a **dictionary comprehension** when assigning the crew positions/members to
      the ship's "crew_members" key.

      :exclamation: If necessary write a `for` loop that adds the position/crew member key-value pairs
      to a dictionary named `crew_members`. Get it working first and then convert it to a dictionary
      comprehension.

      :bulb: Avoid looping over the passed in lists. Instead **loop over a sequence of numbers** and
      think carefully about the appropriate stop value to pass to the `range` type as well as a
      conditional statement inside the loop block that ensures that the passed in ship's "crew_size"
      constraint is not violated. Utilize the sequence of numbers to pair `crew_position` and
      `crew_members` elements by their matching index values.

   2. The crew positions (e.g., 'pilot', 'copilot') and personnel (e.g., Anakin Skywalker, Obi-Wan
      Kenobie) must be passed in separate `crew_positions` and `crew_members` tuples to the function
      `assign_crew_members()`.

   3. Assume that each ship requires a full compliment of personnel to crew the vessel. In other words,
      if a starship's "crew_size" equals three (`3`) you _must_ pass to `assign_crew_members()`
      three crew positions and three personnel.

      Assume too that the maximum number of crew members that can be assigned to a starship is limited
      by the starship's "crew_size" value. No additional crew members are permitted to be assigned to
      the starship even if included in the `crew_positions` and `crew_members` tuples. This limitation
      _must_ be imposed from __within__ the dictionary comprehension.

      For example, if a starship's "crew_size" value equals `3` but `4` crew positions/personnel are
      passed to the function only the first `3` crew positions/personnel are permitted to be added
      as key-value pairs to the crew members dictionary.

      :exclamation: The passed in `crew_positions` and `crew_members` tuples _must_ contain the same
      number of items. The individual `crew_positions` and `crew_members` items are then paired by
      index position and stored in a dictionary structured as follows:

      ```python
      {< crew_position[0] >: < crew_member[0] >, < crew_position[1] >: < crew_member[1] >, ...}
      ```

2. Passengers

   1. You _must_ employ either **a list comprehension or list slicing** when assigning the passengers
      to the ship's "passenger_on_board" key.

   2. The number of passengers permitted to board a ship is limited by the passed in `max_passengers`
      value. If the number of passengers attempting to board exceeds `max_passengers` only the first
      `n` passengers (where `n` = `max_passengers`) are permitted to board the vessel.

      For example, if a starship's `max_passengers` value equals `10` and `20` passengers attempt to
      board the starship, only the first `10` passengers are permitted aboard the vessel.

3. After assigning the crew members and boarding passengers (if any), return `ship` to the caller.

### 3.14.2 Test `board_ship()`

After implementing `board_ship()` return to `main()`.

1. Create a dictionary representation of the Galactic senator
   [Padmé Amidala](https://starwars.fandom.com/wiki/Padm%C3%A9_Amidala). Pass the senator's name
   as a `params` value. Utilize the same "creational" workflow employed to create the dictionary
   representations of `anakin` and `obi_wan`. Use the following variable names to represent
   Padmé Amidala.

   * `swapi_padme` (assigned to the SWAPI dictionary)
   * `wookiee_padme` (assigned to the Wookieepedia dictionary)
   * `padme` (assigned to the `transform_sentient_being()` return value)

2. Call the function `utl.write_json()` and write `padme` to the file `stu-padme_amidala.json`.
   Compare your file to the test fixture file `fxt-padme_amidala.json`. Both files _must_ match
   line for line, indent for indent, and character for character.

3. Create a dictionary representation of the protocol droid named
   [C-3PO](https://starwars.fandom.com/wiki/C-3PO). Pass the droid's name as a `params` value.
   Utilize the same "creational" workflow employed to create `r2_d2`. Consider using the following
   variable names to represent C-3PO.

   * `swapi_c_3po` (assigned to the SWAPI dictionary)
   * `wookiee_c_3po` (assigned to the Wookieepedia dictionary)
   * `c_3po` (assigned to the `transform_droid()` return value)

4. Call the function `utl.write_json()` and write `c_3po` to the file `stu-c_3po.json`. Compare your
   file to the test fixture file `fxt-c_3po.json`. Both files _must_ match line for line, indent for
   indent, and character for character.

5. Create a `pathlib.Path` object that provides an **absolute** path to the file
   "data-jedi.json". Assign the object to a variable named `jedi_path`.

   :bulb: The file "data-jedi.json" provides a JSON array of select members of the
   [Jedi Order](https://starwars.fandom.com/wiki/Jedi_Order). The array includes
   [Ahsoka Tanu](https://starwars.fandom.com/wiki/Ahsoka_Tano),
   [Mace Windu](https://starwars.fandom.com/wiki/Mace_Windu),
   [Plo Koon](https://starwars.fandom.com/wiki/Plo_Koon),
   [Shaak Ti](https://starwars.fandom.com/wiki/Shaak_Ti),
   and [Yoda](https://starwars.fandom.com/wiki/Yoda).

6. Call the function `utl.read_json()` passing it the argument `jedi_path`. Assign the return
   value to a variable named `jedi`.

7. Unpack the `jedi` list into the following variables in the order specified:

   1. `ahsoka_tanu`
   2. `mace_windu`
   3. `plo_koon`
   4. `shaak_ti`
   5. `yoda`

8. Create a tuple literal that contains `anakin`, `obi_wan`, and `mace_windu`.  Assign the variable
   `test_crew_members` to the tuple.

9. Create a tuple literal that contains the strings "pilot", "copilot", "navigator".  Assign the
   variable `test_crew_positions` to the tuple.

10. Create a list literal that contains `padme`, `c_3po`, `r2_d2`, `ahsoka_tanu`, `plo_koon`,
   `shaak_ti`, and `yoda`.

11. Call the functiion `board_ship()` and pass the `test_*` variables to it as arguments. Assign the
    variable `test_ship` to the return value of the function call.

12. Uncomment the two `assert` statements and test your implementation of `board_ship()`.
   Run your code. If an `AssertionError` is raised, debug your code, and then retest. Repeat as
   necessary.

   :bulb: If the function `board_ship()` is implemented correctly the ship's "crew_size" and
   "max_passengers" values will impose limits on the number crew members and passengers that are
   permitted aboard the vessel.

### 3.14.3 Board the Twilight

Call the function `board_ship()` and pass it the following arguments in the order specified:

1. `twilight`
2. a tuple literal containing `anakin` and `obi_wan`
3. a tuple literal containing the strings "pilot" and "copilot"
4. a list literal containing `padme`, `c_3po`, `r2_d2`

Assign the variable `twilight` to the return value of the function call.

### 3.14.4 Issue instructions to R2-D2

Create a list literal containing Anakin's "Power up the engines" order (a string) and map (i.e.,
assign) the list to `r2_d2's` "instructions" key.

## 3.15 Challenge 15 (115 points)

> R2 release the docking clamp. _Anakin Skywalker_

**Task**: With our heroes aboard the _Twilight_ and the engines fired, Anakin Skywalker orders
R2-D2 to chart a course to the planet Naboo. He then orders R2-D2 to release the docking clamp and
detach the light freighter _Twilight_ from the stricken heavy cruiser _Malevolence_. The _Twilight_
then departs to rejoin the Republican fleet before heading to Naboo. Simulate the escape by updating
`r2_d2's` "instructions" and writing the `twilight` dictionary to a file.

### 3.15.1 Issue additional instructions to R2-D2

1. Call the function `utl.get_nested_dict()` and pass it the following arguments: `planets`,
   `diameter_km`, and the integer `12120`. Assign the return value to a variable named `naboo`.

2. Access the `naboo` dictionary's "region" and "sector" values and include the names in a formatted
   string literal (f-string) structured as follows:

   ```python
   "Plot course for Naboo, < region >, < sector >"
   ```

3. Add the f-string to `r2_d2's` "instructions" list so that _Twilight_ can be directed to the
   planet [Naboo](https://starwars.fandom.com/wiki/Naboo), Padmé Amidala's home world.

### 3.15.2 Release the docking clamp

Add Anakin's order "Release the docking clamp" to `r2_d2's` "instructions" list.

### 3.15.3 Escape from the Malevolence

Simulate the _Twilight's_ escape from the _Malevolence_ by writing the `twilight` dictionary to the
file `stu-twilight_departs.json`. Run your code. Compare your file to the test fixture file
`fxt-twilight_departs.json`. Both files _must_ match line for line, indent for indent, and
character for character.

Assuming that the JSON files match submit your code to Gradescope. If you earn full points your work
is done. Never mind that Separatist starfighters are in hot pursuit of the _Twilight_&mdash;declare
victory!

Congratulations on completing SI 506.

## FINIS :tada:
