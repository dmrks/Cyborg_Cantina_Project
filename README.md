# Cyborg_Cantina_Project

Cyborg Cantina
Retrieval-based chatbots are commonly used in customer service environments. User questions are constrained to the specific service being provided, 
which approximates a closed-domain conversation. However, people are known to be especially picky with their food. 
Can you create a chatbot system that effectively answers all of a diner’s questions about the food on a restaurant menu?

In this project you will build a retrieval-based chatbot system for a restaurant serving Mexican cuisine. 
You’ll use a combination of tf-idf scoring, word embedding models, and a set of use-defined functions in order to answer any number of questions from a restaurant diner.

By the end of this project, you’ll have constructed a full retrieval-based chatbot system, conversed with your own chatbot creation, and improved the system by adding additional candidate responses!


# Chatbot Input/Output
1.
Let’s begin at the end! We must provide a way for our user to end a conversation once they have had all their questions answered. Note that exit_commands,
a list of strings commonly used as exit commands from a chatbot system, is already defined in the workspace.

Define a .make_exit() method with self and user_message as parameters.
Within .make_exit(), write a for loop over each object in exit_commands.
For each object in exit_commands, check if the object also occurs in user_message.
If the object does occur in user_message, print a goodbye message to the console, and return True.
If the object does not occur in user_message, do nothing.

2.
Of course, we should provide a method that allows our chatbot to chat:

Define a .chat() method with self as a parameter.
Within the .chat() method, write a welcoming prompt for a user question, using the input() function. Set the result equal to user_message.
Create a while not loop that checks whether .make_exit(user_message) is True at each iteration.
Within the while loop, call .respond() on user_message and assign the result to user_message.


3.
Let’s test out the .make_exit() functionality of the bot:

Outside of the ChatBot class, initialize a ChatBot instance.
Call the .chat() method on the ChatBot instance.
Run python3 script.py in the console to start your chatbot!
Ask a question that includes one of the terms in exit_commands.

# Intent Classification

4.
We’ve already imported a collection of functions created throughout the Retrieval-based Chatbots lesson into the workspace, as well as a set of pre-defined responses for our retrieval-based bot.

Check out the user_functions.py file to refresh your understanding of the preprocess(), compare_overlap(), extract_nouns(), and compute_similarity() functions.
Look over the responses.py file to see the collection of responses already written for our bot.

5.
Let’s build a set of BoW models from our data:

Define a .find_intent_match() method with self, responses, and user_message as parameters.
In the body of .find_intent_match(), call preprocess() on user_message, then call Counter() on the result to create a bag-of-words (BoW) model.
Call preprocess() on each item in responses, then call Counter() on the result.


6.
Now we can select the response that best matches the intent of the user message:

Still in the body of .find_intent_match(), apply compare_overlap() on each response in processed_responses. Save the resulting list item to similarity_list.
Use Python’s .index() method and max() function to select the index of the highest similarity score in similarity_list. Save the result to response_index.
Use list indexing to return the element at index response_index in responses.

7.
Let’s test our .find_intent_match() method:

Define a method called .respond(), with self and user_message as parameters.
Assign the result of calling .find_intent_match(responses, user_message) to a variable called best_response.
Within .respond(), print best_response to the console.
To allow for multiple questions, use the input() function to prompt the user for another question. Assign the result to input_message.
Return input_message.
Run your script in the terminal to check if a response is returned!


# Entity Recognition

8.
Let’s extract candidate entities from the user message:

Define a .find_entities() method with self and user_message as parameters.
In the body of .find_entities(), call preprocess() on user_message. Then call pos_tag() on the result.
Call extract_nouns() on tagged_user_message. Save the result to message_nouns.


9.
Now we can fit a word2vec model on our candidate entities:

Use " ".join() to create a concatenated string from message_nouns. Call word2vec() on this string. Save the result to a variable called tokens.
Call word2vec() on blank_spot. Save the result to a variable called category.
Call compute_similarity() on tokens and category. Save the result to word2vec_result.

10.
Finally, let’s select the entity with the highest similarity score:

Call sort(key=lambda x: x[2]) on word2vec_result. This will sort the result list by ascending similarity score.
Write an if statement to check whether word2vec_result has at least one item. If False, return blank_spot. Otherwise, return the first element of the last list item in word2vec_result.


# Response Selection

11.
Let’s pull together the results from the Intent Classification and Entity Extraction tasks:

Within .respond(), delete the line which prints best_response to the console.
Directly after the call to .find_intent_match, assign the result of calling .find_entities(user_message) to entity.
Call Python’s .format() method, with entity as an argument, on best_response. Print the result to the terminal.
Call your script from the terminal to test out your bot!
