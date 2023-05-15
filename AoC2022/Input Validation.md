The Story  

![Banner for Day 17](https://tryhackme-images.s3.amazonaws.com/user-uploads/60c1834f577d63004fdaec50/room-content/d05e1c8db9484ef8201231659ce26b0d.png)

Check out InsiderPhD's video walkthrough for Day 17 [here](https://www.youtube.com/watch?v=ZsmRQqjGb9E)!  

After handling unrestricted file uploads and SQLi vulnerabilities, McSkidy continued to review Santa's web applications. She stumbled upon user-submitted inputs that are unrecognizable, and some are even bordering on malicious! She then discovered that Santa's team hadn't updated these web applications in a long time, as they clearly needed more controls to filter misuse. Can you help McSkidy research and learn a useful technique to handle that in the future? 

Introduction

At this point, we are already well aware of the security concerns regarding input validation. Analogous to being the castle wall of your application that forms the first line of defense against an array of scenarios from harmless misuse to outright malicious intents, validating input merits the same effort as its medieval counterpart in terms of its hardening.

It goes without question that insufficient effort in this space may result in attack vectors being vulnerable and consequently exploited in your application. Day 15 talked about input validation in light of Unrestricted File Upload, while Day 16 focused on SQLi.

In this task, we will be exploring input validation in a more general sense before touching upon one of its most complex use cases: free-form text fields, followed by a quick discussion on how you may move forward in your secure coding journey beyond input validation.

Input Validation Foundations

Generally, an effective way to validate input is first to know how a specific piece of data is going to be processed by the rest of your application. The Day 15 task is a beautiful example that shows more or less the mindset required to be able to effectively tackle the specific case of Unrestricted File Upload.

Data then goes through syntax and semantic validation checks to ensure that the user-provided values are both proper in their syntax (the answer follows the proper context asked by the question) and logical value (the values make sense for the question).

Going back to Day 15:

1.  You cannot  manually type your CV in the input field - the form asks for a file, so it’s simply not what’s being asked
2.  You cannot just upload any file - it should follow a set of very specific rules, the implementation of which was all discussed in the latter parts of the task.

Then comes whitelisting, where you can be very specific with what your forms would accept and immediately strip or even drop the ones that don’t fit in the predefined allowed category.

HTML5 and Regex

HTML5’s built-in features help a lot with the validation of user-provided input, minimizing the need to rely on JavaScript for the same objective. The `<input>` element, specifically has an array of very helpful capabilities centered around form validation.

For instance, the `<input>` type, which can be set to specifically filter for an email, a URL, or even a file, among others, promptly checks whether or not the user-provided input fits the type of data that the form is asking for, and so, feedback on its validity is immediately returned to the user as a result.

For even more granular control of the input being provided, regular expressions (regex) can be integrated into the mix. Simply use it in the "pattern" attribute within the `<input>` element, and you’re all set. [Here](https://www.regular-expressions.info/quickstart.html) is a nice resource to get started with regular expressions. A couple of examples are shown below.

`1. <input type="text" id="uname" name="uname" pattern="[a-zA-Z0-9]+">   2. <input type="email" id="email" name="email" pattern=".+@tryhackme\.com">`

The pattern in the first line of code above is easily one of the most foundational regular expression patterns one can use. The instruction here is to match any strings specifically composed of only letters and numbers - an alphanumeric pattern that is case-insensitive.

The pattern in the second line of code above is a bit more pointed in its instruction, specifying that an email can have any characters at the beginning as long as it ends with "@tryhackme.com".

Developing regular expressions can be very daunting as its nature is complex; however its capability to match very specific patterns is what makes it special. Well-built regular expressions introduce a great way to immediately filter out user-provided input that doesn't fit the specific requirements that you have set. 

Regex 101

This section will talk about some regex tips to get you started. To match _any lowercase_ character from the English alphabet, the regex pattern is `[a-z]`. We can deconstruct it as follows:

-   The square brackets indicate that you're trying to match _one character_ within the set of characters inside of them. For example, if we're trying to match any vowel of the English alphabet, we construct our regex as follows: `[aeiou]`. The order of the characters doesn't matter, and it will match the same.
-   Square brackets can also accept a range of characters by adding a hyphen, as seen in our original example.
-   You can also mix and match sets of characters within the bracket. `[a-zA-Z]` means you want to match any character from the English alphabet regardless of case, while `[a-z0-9]` means you want to match any lowercase alphanumeric character.

We also need to talk about regex operators. The simplest one is the wildcard operator, denoted by `.` . This means regex will match _any_ character, and it's quite powerful when used with the operators `*`, `+`, and `{min,max}`. The asterisk or star operator is used if you don't care if the preceding token matches anything or not, while the plus operator is used if you want to make sure that it matches at least once. The curly braces operator, on the other hand, specifies the number of characters you want to match. Let's look at the following examples:

-   To match a string that is alphanumeric and case insensitive, our pattern would be `[a-zA-Z0-9]+`. The plus operator means that we want to match a string, and we don't care how long it is, as long as it's composed of letters and numbers regardless of their case.
-   If we want to ensure that the first part of the string is composed of letters and we want it to match regardless if there are numbers thereafter, it would be `^[a-zA-Z]+[0-9]*$`. The `^` and `$` operators are called anchors, and denote the start and end of the string we want to match, respectively. Since we wanted to ensure that the start of the string is composed of only letters, adding the caret operator is required.
-   If we want to match just lowercase letters that are in between 3 and 9 characters in length, our pattern would be `^[a-z]{3,9}$`.
-   If we want a string that starts with 3 letters followed by _any_ 3 characters, our pattern would be `^[a-zA-Z]{3}.{3}$`.

There's also the concept of grouping and escaping, denoted by the `()` and the `\` operators, respectively. Grouping is done to manage the matching of specific parts of the regex better while escaping is used so we can match strings that contain regex operators. Finally, there's the `?` operator, which is used to denote that the preceding token is optional. Let's look at the following example:

-   If we want to match both _www.tryhackme.com_ and _tryhackme.com_, our pattern would be `^(www\.)?tryhackme\.com$`. This pattern would also avoid matching _.tryhackme.com._
-   `^(www\.)?`: The `^` operator marks the start of the string, followed by the grouping of www and the escaped `.`, and immediately followed by the question mark operator. The grouping allowed the question mark operator to work its magic, matching both strings with or without the _www._ at the beginning.
-   `tryhackme\.com$`: The `$` operator marks the end of the string, preceded by the string tryhackme, an escaped `.`, and the string com. If we don't escape the `.` operator, the regex engine will think that we want to match any character between tryhackme and com as well.

It's also imperative to note that the wildcard operator can lead to laziness and, consequently misuse. As such, it's always better to use character sets through the brackets especially when we're validating input as we want it to be perfect.

Here's a table to summarize everything above:

[]

Character Set: matches any single character/range of characters inside

.

Wildcard: matches any character

*

Star / Asterisk Quantifier: matches the preceding token zero or more times

+

Plus Quantifier: matches the preceding token one or more times

{min,max}

Curly Brace Quantifier: specifies how many times the preceding token can be repeated

()

Grouping: groups a specific part of the regex for better management

\

Escape: escapes the regex operator so it can be matched

?

Optional: specifies that the preceding token is optional

^

Anchor Beginning: specifies that the consequent token is at the beginning of the string

$

Anchor Ending: specifies that the preceding token is at the end of the string

The Unique Case of Free-Form Text

All of the techniques that we have covered in this task thus far are mainly concerned with structured data - data that we already expect what it should look like. However, compared to the validation of structured data, free text is more complex and not very straightforward.  

Structured data have inherent characteristics that allow us to set them apart quite quickly. For example, names shouldn’t consist of special characters (some names have, but these can be whitelisted), and age should only consist of numbers with an absolute maximum of 3 characters. Syntax and semantic validation checks can be immediately done on them, and the chaos suddenly doesn’t seem too bad.

In contrast, free text fields are more free-for-all, and so validations checks are more limited, and the challenge of securing it is generally vaguer. Yet, like all great engineers before us, we power through these challenges and make the best with what we’re given. Listed below are some considerations to ponder on.

-   First, we start again with the question of how this piece of data is going to be processed by the rest of the application.
-   What will be the context for which this free-form text field will be used? Free text fields for blog posts are tackled very differently than text fields used for a small comment section or a descriptive text field.
-   Is the free text field necessary for your business purposes in the first place, or could it be implemented differently while still achieving the same goal? This is essential to know, too, as part of writing secure code is avoiding writing vulnerable ones!

Then we go to the tricky part. Since syntax and semantic checks are pretty much impossible due to the nature of free-form text fields, our best bet in having a bit of control is through whitelisting. This [OWASP Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html#validating-free-form-unicode-text) lists down the general techniques to perform whitelisting, which can be summarized as follows:

1.  Ensure no invalid characters are present through proper encoding, and
2.  Whitelist expected characters and character sets

Best Remediation Tactic  

For the next part of the discussion, let’s take a look at the simple base code below wherein a programmer attempts to retrieve the usual details from a user.

![Base Code](https://tryhackme-images.s3.amazonaws.com/user-uploads/60c1834f577d63004fdaec50/room-content/944be37798d52b534749b314d6c90849.png)  

The example is a bit bare and would have been an immediate nightmare as it raises many alarms not only in terms of security, but also in terms of risking getting a ton of garbage responses. Since all of them are text inputs by default, the birthday and age fields may receive responses that aren’t within their specific context. Further, malicious users have free reign over the fields, essentially allowing them a free playground to tinker with.

This is the case if no input validation is done at all. Let’s fix it up a bit and review it again.

![Edited Base Code to Include Type Attribute](https://tryhackme-images.s3.amazonaws.com/user-uploads/60c1834f577d63004fdaec50/room-content/a7484e2056785655394786a397857a5f.png)  

The changes above are literally just additions of the _type_ attribute and their corresponding values within the input element. Yet, there’s already a world of difference between this one and the base implementation prior. Take note that security wasn’t the main focus of these changes, yet it succeeds in mainly alleviating the chaos that the prior implementation otherwise invited. Going back to the discussion above, this is an example of the syntax check being done in the HTML layer.

However, the implementation above is still susceptible to misuse, especially for very naughty users. Let’s fix it up further and review it again.

![Further Edit of Base Code to Include Semantic and Whitelisting Techniques](https://tryhackme-images.s3.amazonaws.com/user-uploads/60c1834f577d63004fdaec50/room-content/4b0203a837d72f92684f35c8bd1d00b6.png)  

The changes done above incorporate both semantic checking and whitelisting techniques to the existing syntax checking in the prior implementation in order to further filter out allowed values that can be provided in the input fields.

1.  The name field, for instance, has been written to allow a maximum of 70 characters - still quite long but somewhat reasonable. This can further be filtered granularly by blacklisting numbers and special symbols, but for our purposes, let’s accept this one for now.
2.  The date field is tweaked, so the earliest date it would allow is in the year 1900, while the latest date is in 2014, with the working assumption that the youngest user of the website would be eight years old.
3.  The email field has also been revamped to only accept tryhackme emails, while the URL field is further narrowed down to only accept tryhackme rooms as the answer to the ‘favewebsite’ field.

These semantic checks should follow the proper business context to be effective. For example, the implementation above is geared towards the tryhackme staff, marked by the email field only accepting tryhackme emails. Furthermore, applying the foundations of input validation not only ensures that the data being provided is proper, but it also drastically limits the attack surface of the input fields, especially those requiring structured data.

This is the reason why we haven’t touched the `<textarea>` field at the very end of the code block. As discussed earlier, free-form text is unique in such a way that applying syntax and semantic checks are pretty much impossible. In order to secure this one, we would first need to define what content (e.g. characters, character sets, symbols, etc.) is acceptable and then perform the actual whitelisting. Even then, it wouldn’t be enough as it’s still a very open field, and so techniques such as output escaping and using parametrized queries, among others, should be implemented as well.

Client-side vs Server-side

Our discussion until this point has been geared towards using input validation to ensure that the pieces of data that we're getting from the users are not garbage. Proper input validation on the client-side limits misuse and minimizes the attack surface of the application, however, it doesn't actively cover malicious use cases.  

As opposed to the above examples of validating input implemented on the client-side, output escaping and using parametrized queries are implemented on the server-side, adding computational load to the server, but ultimately helping in securing the application as a whole. This is done as an exercise of inherent distrust in the user-provided input despite validation. 

The example below uses the built-in `htmlentities()` PHP function to escape any character that can affect the application.

`$name = htmlentities($_GET['name'], ENT_QUOTES | ENT_HTML5, "UTF-8")`

On the other hand, the one below uses the HTMLPurifier library to help with some use cases, such as our `<textarea>` conundrum above. 

![Purifier Example](https://tryhackme-images.s3.amazonaws.com/user-uploads/60c1834f577d63004fdaec50/room-content/f66f8bfc285d5d43cfcaea4834ff6adb.png)

Both examples are done on the server-side to escape characters and purify content, respectively both effective ways to prevent user misuse and XSS attacks. Implementing both client and server-side validations ensure that no stones are left unturned.

This exact case highlights the importance of layering defenses and shows point-blank the limitation of client-side input validation. It also gives rise to the notion that input validation is not a one-stop shop for securing your application.

Specific cases warrant specific security requirements, and so on this factor alone, it’s already apparent that there exist multiple ways of attacking the challenge of securing user-provided input, and most of the time, they are implemented on top of each other.

Not All Solutions are Equal

Input validation is an important layer of security that all production-level applications should have. However, no matter how ‘perfect’ your input validation is for your specific use case, there simply are limitations to all kinds of security implementations - and input validation is not exempt from it.

You cannot make your application fully XSS-proof through input validation alone. Controls may be put in place at the input level that may lessen the attack surface of the application, but it doesn’t fully remediate it. Full remediation of an XSS vulnerability in your application requires additional layers of defenses, such as mitigation on the browser-level (via secure cookies, content-security-policy headers, etc.) and escaping and purifying user-provided input.

In light of these remediation techniques, it makes sense to tackle each challenge on its own - SQL and LDAP-related vulnerabilities are addressed differently from XSS, so why try to fit them all in the same formula?  

Summary

Despite being a very important security control, due diligence in securing applications involving user-provided input should not stop in validating the input per se. As seen in previous examples, user input may be valid, but it doesn’t necessarily mean that it’s harmless.

There are a lot of tools and frameworks out there that when used properly can help secure your application. For instance, HTML5's features can help in minimizing input form misuse, while the use of regular expressions can help filter out what we need from the user more granularly. We took a closer look at regex here especially because of its powerful capability to implement filters.

And while the above examples aren't directly concerned about application security, they help minimize the attack surface of the application, which is further tackled through server-side validation techniques such as output escaping and using parametrized queries.  

We should be realistic and not try to solve all security problems with input validation. There is not one control that could prevent persistent malicious actors and so layering these controls is the better way to move forward.