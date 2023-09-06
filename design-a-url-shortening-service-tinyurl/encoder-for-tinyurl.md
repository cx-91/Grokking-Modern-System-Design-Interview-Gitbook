# Encoder for TinyURL

### Introduction <a href="#introduction-0" id="introduction-0"></a>

We've discussed the overall design of a short URL generator (SUG) in detail, but two aspects need more clarification:

1. How does encoding improve the readability of the short URL?
2. How are the sequencer and the base-58 encoder in the short URL generation related?

#### Why to use encoding <a href="#why-to-use-encoding-1" id="why-to-use-encoding-1"></a>

Our sequencer generates a 64-bit ID in base-10, which can be converted to a base-64 short URL. Base-64 is the most common encoding for alphanumeric strings' generation. However, there are some inherent issues with sticking to the base-64 for this design problem: the generated short URL might have readability issues because of look-alike characters. Characters like `O` (capital o) and `0` (zero), `I` (capital I), and `l` (lower case L) can be confused while characters like `+` and `/` should be avoided because of other system-dependent encodings.

So, we slash out the six characters and use base-58 instead of base-64 (includes A-Z, a-z, 0-9, `+` and `/`) for enhanced readability purposes. Let's look at our base-58 definition.

### Base-58

| Value | Character | Value | Character | Value | Character | Value       | Character   |
| ----- | --------- | ----- | --------- | ----- | --------- | ----------- | ----------- |
| 0     | 1         | 15    | G         | 30    | X         | 45          | n           |
| 1     | 2         | 16    | H         | 31    | Y         | 46          | o           |
| 2     | 3         | 17    | J         | 32    | Z         | 47          | p           |
| 3     | 4         | 18    | K         | 33    | a         | 48          | q           |
| 4     | 5         | 19    | L         | 34    | b         | 49          | r           |
| 5     | 6         | 20    | M         | 35    | c         | 50          | s           |
| 6     | 7         | 21    | N         | 36    | d         | 51          | t           |
| 7     | 8         | 22    | P         | 37    | e         | 52          | u           |
| 8     | 9         | 23    | Q         | 38    | f         | 53          | v           |
| 9     | A         | 24    | R         | 39    | g         | 54          | w           |
| 10    | B         | 25    | S         | 40    | h         | 55          | x           |
| 11    | C         | 26    | T         | 41    | i         | 56          | y           |
| 12    | D         | 27    | U         | 42    | j         | 57          | z           |
| 13    | E         | 28    | V         | 43    | k         | <p><br></p> | <p><br></p> |
| 14    | F         | 29    | W         | 44    | m         | <p><br></p> | <p><br></p> |

> The highlighted cells contain the succeeding characters of the omitted ones: `0`, `O`, `I`, and `l`.

### Converting base-10 to base-58 <a href="#converting-base-10-to-base-58-0" id="converting-base-10-to-base-58-0"></a>

Since we're converting base-10 numeric IDs to base-58 alphanumeric IDs, explaining the conversion process will be helpful in grasping the underlying mechanism as well as the overall scope of the SUG. To achieve the above functionality, we use the **modulus** function.

**Process**: We keep diving the base-10 number by 58, making note of the remainder at each step. We stop where there is no remainder left. Then we assign the character indexes to the remainders, starting from assigning the recent-most remainder to the left-most place and the oldest remainder to the right-most place.

**Example**: Let's assume that the selected unique ID is `2468135791013`. The following steps show us the remainder calculations:

Base-10 = 2468135791013

1.   2468135791013 % 58=172468135791013 % 58=17
2.   42554065362 % 58=642554065362 % 58=6
3.   733690782 % 58=4733690782 % 58=4
4.   12649841 % 58=4112649841 % 58=41
5.   218100 % 58=20218100 % 58=20
6.   3760 % 58=483760 % 58=48
7.   64 % 58=664 % 58=6
8.   1 % 58=11 % 58=1

Now, we need to write the remainders in order of the most recent to the oldest order.

Base-58 = \[1] \[6] \[48] \[20] \[41] \[4] \[6] \[17]

Using the table above, we can write the associated characters with the remainders written above.

Base-58 = 27qMi57J

> **Note:** Both the base-10 numeric IDs and base-64 alphanumeric IDs have 64-bits, as per our sequencer design.

Let's see the example above with the help of the following illustration.

![](<../.gitbook/assets/Screenshot 2023-09-06 at 1.05.08 AM.png>)

### Converting base-58 to base-10 <a href="#converting-base-58-to-base-10-0" id="converting-base-58-to-base-10-0"></a>

The decoding process holds equal importance as the encoding process, as we used a decoder in case of custom short URLs generation, as explained in the [design lesson](design-and-deployment-of-tinyurl.md).

**Process**: The process of converting a base-58 number into a base-10 number is also straightforward. We just need to multiply each character index (value column from the table above) by the number of 58s that position holds, and add all the individual multiplication results.

**Example**: Let's reverse engineer the example above to see how decoding works.

<figure><img src="../.gitbook/assets/Screenshot 2023-09-06 at 1.05.48 AM.png" alt=""><figcaption></figcaption></figure>

This is the same unique ID of base-10 from the previous example.

Let's see the example above with the help of the following illustration.

<figure><img src="../.gitbook/assets/Screenshot 2023-09-06 at 1.06.06 AM.png" alt=""><figcaption></figcaption></figure>

### The scope of the short URL generator <a href="#the-scope-of-the-short-url-generator-0" id="the-scope-of-the-short-url-generator-0"></a>

The short URL generator is the backbone of our URL shortening service. The output of this short URL generator depends on the design-imposed limitations, as given below:

* The generated short URL should contain alphanumeric characters.
* None of the characters should look alike.
* The minimum default length of the generated short URL should be six characters.

These limitations define the scope of our short URL generator. We can define the scope, as shown below:

<figure><img src="../.gitbook/assets/Screenshot 2023-09-06 at 1.07.05 AM.png" alt=""><figcaption></figcaption></figure>

> **Maximum digits**: The calculations above show that the maximum digits in the sequencer generated ID will be 20 and consequently, the maximum number of characters in the encoded short URL will be 11.  

**Question 1**

Since we’re using the 10 digits and beyond sequencer IDs, is there a way we can use the sequencer IDs shorter than 10 digits?

We can use the range below the ten digits sequencer IDs for custom short links for users with premium memberships. It will ensure two benefits:

* Utilization of the blocked range of IDs
* Less than six characters short URLs

**Example**: Let’s assume that the user requests `abc` as a custom short URL, and it’s available in our system, as there is no instance in the data store matching with this short URL. We need to perform the following two operations:

1. Assign this short URL to the requested long URL and store this record in the datastore.
2. Mark the associated unique ID unusable. To find the associated unique ID, we need to decode `abc` into base-10. Using the above decode method, we come up with the base-10 unique ID value as `113019`. The unique ID is less than 1 Billion, as the custom short URL is less than six characters, conforming to the above-stated two benefits.

> Our system doesn’t ensure a guaranteed custom short link generation, as some other premium member might have claimed the requested custom short URL.

**Question 2**

What should the short URL be for the sequencer’s largest number?

Hide Answer

<figure><img src="../.gitbook/assets/Screenshot 2023-09-06 at 1.07.55 AM (1).png" alt=""><figcaption></figcaption></figure>

\----------------------

### The sequencer's lifetime <a href="#the-sequencers-lifetime-0" id="the-sequencers-lifetime-0"></a>

The number of years that our sequencer can provide us with unique IDs depends on two factors:

<figure><img src="../.gitbook/assets/Screenshot 2023-09-06 at 1.10.29 AM.png" alt=""><figcaption></figcaption></figure>

&#x20;[_Requirements_](requirements-of-tinyurls-design.md)

So, taking the above two factors into consideration, we can calculate the expected life of our sequencer.

<figure><img src="../.gitbook/assets/Screenshot 2023-09-06 at 1.09.16 AM.png" alt=""><figcaption></figcaption></figure>

**Life expectancy for sequencer**

| Number of requests per month | 200            | Million |
| ---------------------------- | -------------- | ------- |
| Number of requests per year  | f2.4           | Billion |
| Lifetime of sequencer        | f7686143363.63 | years   |

> Therefore, our service can run for a long time before the range depletes.
