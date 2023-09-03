# System Design: The Typeahead Suggestion System

### Introduction <a href="#introduction-0" id="introduction-0"></a>

**Typeahead suggestion**, also referred to as the **autocomplete feature**, enables users to search for a known and frequently searched query. This feature comes into play when a user types a query in the search box. The typeahead system provides a list of suggestions to complete a query based on the user’s search history, the current context of the search, and trending content across different users and regions. Frequently searched queries always appear at the top of the suggestion list. The typeahead system doesn’t make the search faster. However, it helps the user form a sentence more quickly. It’s an essential part of all search engines that enhances the user experience.

The typeahead suggestion system in action

### How will we design a typeahead suggestion system? <a href="#how-will-we-design-a-typeahead-suggestion-system-0" id="how-will-we-design-a-typeahead-suggestion-system-0"></a>

We’ve divided the chapter on the design of the typeahead suggestion system into six lessons:

1. [**Requirements**](https://www.educative.io/collection/page/10370001/4941429335392256/6369391511339008)**:** In this lesson, we focus on the functional and non-functional requirements for designing a typeahead suggestion system. We also discuss resource estimations for the smooth operation of the system.
2. [**High-level design**](https://www.educative.io/collection/page/10370001/4941429335392256/6376843531255808)**:** In this lesson, we discuss the high-level design of our version of the typeahead suggestion system. We also discuss some essential APIs used in the design.
3. [**Data Structure for Storing Prefixes**](https://www.educative.io/collection/page/10370001/4941429335392256/6110217372172288)**:** In this lesson, we cover an efficient tree data structure called trie that’s used to store search prefixes. We also discuss how it can be further optimized to reduce the tree traversal time.
4. [**Detailed design**](https://www.educative.io/collection/page/10370001/4941429335392256/5464491722801152)**:** In this lesson, we explain the two main components, the _suggestions_ service and the _assembler_, which make up the detailed design of the typeahead suggestion system.
5. [**Evaluation**](https://www.educative.io/collection/page/10370001/4941429335392256/6626894497185792)**:** This lesson evaluates the proposed design of the typeahead suggestion system based on the non-functional requirements of the system. It also presents some client-side optimization and personalization that could significantly affect the system’s design.
6. [**Quiz**](https://www.educative.io/collection/page/10370001/4941429335392256/5201949791617024)**:** In this lesson, we assess our understanding of the design via a quiz.

Let’s start by identifying the requirements for designing a typeahead suggestion system.
