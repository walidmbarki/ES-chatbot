# ES-chatbot
An academic project for emotional support chatbot purely and uniquely made of c++



#include <iostream>
#include <string>
#include <vector>
#include <map>
#include <cstdlib>
#include <ctime>
#include <cctype>

using namespace std;

//naj et minuscule

string toLower(string s) {
    for (size_t i = 0; i < s.length(); i++)
        s[i] = tolower(s[i]);
    return s;
}

bool containsAny(const string& text, const vector<string>& words) {
    for (size_t i = 0; i < words.size(); i++) {
        if (text.find(words[i]) != string::npos)
            return true;
    }
    return false;
}

string randomFrom(const vector<string>& v) {
    return v[rand() % v.size()];
}

//bach ndetektiw random words
bool isKeyboardSmash(const string& input) {
    int vowels = 0;
    for (char c : input) {
        if (c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u')
            vowels++;
    }
    return (input.length() >= 5 && vowels < 2);
}

//main code

int main() {
    srand((unsigned)time(NULL));

    string userName = "";
    bool greeted = false;
    int silenceCount = 0;
    int emotionalLoad = 0;
    string lastBotQuestion = "";   // Context memory

   //the keywords bach y7kem
   
    vector<string> greetings = {
        "hi", "hello", "hey", "heyy", "yo", "sup",
        "good morning", "good evening", "good night"
    };

    vector<string> goodbyes = {
        "bye", "goodbye", "see you", "later", "gn", "goodnight"
    };

    vector<string> nameTriggers = {
        "my name is", "i am", "call me"
    };

    vector<string> negativeReplies = {
        "no", "not", "i am not", "i'm not", "dont", "don't",
        "cannot", "can't", "nah"
    };

    vector<string> jokeTriggers = {
        "joke", "make me laugh", "something funny", "tell me a joke", "funny"
    };

    map<string, vector<string>> emotions;
    emotions["sad"]     = {"sad", "down", "empty", "grief", "hurt"};
    emotions["tired"]   = {"tired", "exhausted", "drained", "burnt out"};
    emotions["stress"]  = {"stress", "stressing", "pressure", "overwhelmed"};
    emotions["anxious"] = {"anxious", "worried", "nervous"};
    emotions["angry"]   = {"angry", "mad", "frustrated", "annoyed"};
    emotions["happy"]   = {"happy", "good", "great", "excited"};

   //l ajwiba
    map<string, vector<string>> responses;

    responses["sad"] = {
        "That sounds really heavy.",
        "I’m sorry you’re carrying that.",
        "That kind of pain builds quietly."
    };

    responses["tired"] = {
        "That exhaustion feels deeper than sleep.",
        "Sounds like you’ve been pushing too hard.",
        "That kind of tired isn’t laziness."
    };

    responses["stress"] = {
        "That’s a lot for one person.",
        "Stress like that adds up fast.",
        "No wonder you feel tense."
    };

    responses["anxious"] = {
        "Anxiety makes everything louder.",
        "Your mind sounds restless.",
        "That constant worry is exhausting."
    };

    responses["angry"] = {
        "That frustration didn’t come from nowhere.",
        "Something crossed a line.",
        "It makes sense you’re upset."
    };

    responses["happy"] = {
        "That’s really nice to hear.",
        "I’m glad something feels good.",
        "That sounds refreshing."
    };

    responses["neutral"] = {
        "I’m listening.",
        "Go on.",
        "Tell me more."
    };

    vector<string> followUps = {
        "Do you want to talk more about it?",
        "How long has this been going on?",
        "What do you think triggered it?",
        "How are you dealing with it?"
    };

    vector<string> jokes = {
        "Why did the computer go to therapy? It had too many bytes of stress.",
        "I told my computer I needed a break, now it won't stop sending me Kit-Kat ads.",
        "Why don’t scientists trust atoms? Because they make up everything!",
        "I’d tell a joke about time travel, but you didn’t like it.",
        "Why was the math book sad? It had too many problems.",
        "Parallel lines have so much in common… it’s a shame they’ll never meet.",
        "Why did the scarecrow win an award? Because he was outstanding in his field!",
        "I’m reading a book about anti-gravity. It’s impossible to put down!",
        "Why do programmers prefer dark mode? Because light attracts bugs.",
        "I told a joke about UDP… but you might not get it.",
        "Why did the coffee file a police report? It got mugged.",
        "What do you call fake spaghetti? An impasta.",
        "Why did the skeleton go to the party alone? He had no body to go with.",
        "I would tell you a joke about infinity… but it never ends.",
        "Why did the computer show up at work late? It had a hard drive.",
        "I’m friends with all electricians… we have good current connections.",
        "Why did the programmer quit his job? He didn’t get arrays.",
        "I told my Wi-Fi a joke… now it’s connected.",
        "Why did the tomato turn red? Because it saw the salad dressing!",
        "I’d tell a joke about recursion… but you might not get it…"
    };

    vector<string> silenceReplies = {
        "Hey… you went quiet.",
        "I’m still here, no rush.",
        "Sometimes silence says a lot."
    };

    vector<string> silenceTopics = {
        "Random question: what music are you into lately?",
        "Are you more into movies or series?",
        "Do you follow any sports?",
        "What usually helps you relax?"
    };

    //the conversation main code
    cout << "Bot: Hey 👋 I’m here. You can talk about anything." << endl;

    string input;
    while (true) {
        cout << "\nYou: ";
        getline(cin, input);
        string lowered = toLower(input);

        // ila makan ta jawab
        if (input.empty()) {
            silenceCount++;
            if (silenceCount >= 2)
                cout << "Bot: " << randomFrom(silenceTopics) << endl;
            else
                cout << "Bot: " << randomFrom(silenceReplies) << endl;
            continue;
        }
        silenceCount = 0;

        //goodbyes
        if (containsAny(lowered, goodbyes)) {
            cout << "Bot: Thanks for talking" << (userName.empty() ? "" : ", " + userName) << ". Take care 🤍" << endl;
            break;
        }

        //les salutations
        if (containsAny(lowered, greetings)) {
            cout << "Bot: Hey" << (userName.empty() ? "" : " " + userName) << " 👋" << endl;
            if (!greeted) {
                cout << "Bot: How are you feeling today?" << endl;
                greeted = true;
                lastBotQuestion = "How are you feeling today?";
            }
            continue;
        }

        //name
        if (containsAny(lowered, nameTriggers) && userName.empty()) {
            size_t pos = lowered.find("is");
            if (pos != string::npos) {
                userName = input.substr(pos + 3);
                cout << "Bot: Nice to meet you, " << userName << " 🤍" << endl;
                continue;
            }
        }

        //time telling
        if (lowered.find("time") != string::npos) {
            time_t now = time(0);
            tm *ltm = localtime(&now);
            cout << "Bot: The current time is "
                 << ltm->tm_hour << ":"
                 << (ltm->tm_min < 10 ? "0" : "") << ltm->tm_min
                 << "." << endl;
            continue;
        }

        //negative energy 
        if (!lastBotQuestion.empty() && containsAny(lowered, negativeReplies)) {
            cout << "Bot: That’s okay. Not coping yet doesn’t mean you’re failing." << endl;
            cout << "Bot: We can take this one step at a time if you want." << endl;
            if (rand() % 2 == 0)
                cout << "Bot: Want help breaking things down together?" << endl;
            lastBotQuestion = "";
            continue;
        }

        //jokes
        if (containsAny(lowered, jokeTriggers)) {
            cout << "Bot: " << randomFrom(jokes) << endl;
            lastBotQuestion = "";
            continue;
        }

        //t9mar words
        if (isKeyboardSmash(lowered)) {
            cout << "Bot: What do you mean with that?" << endl;
            lastBotQuestion = "";
            continue;
        }

        //limite d'emotions
        int detectedEmotionsCount = 0;
        string detectedEmotion = "neutral";
        for (auto it = emotions.begin(); it != emotions.end(); ++it) {
            if (containsAny(lowered, it->second)) {
                detectedEmotionsCount++;
                detectedEmotion = it->first; // first detected
            }
        }

        if (detectedEmotionsCount > 1) {
            cout << "Bot: These are too many emotions, let's focus on how you're feeling at the moment." << endl;
            lastBotQuestion = "";
            continue;
        }

        //Single emotion response
        if (detectedEmotion == "happy")
            emotionalLoad++;
        else if (detectedEmotion != "neutral")
            emotionalLoad--;

        cout << "Bot: " << randomFrom(responses[detectedEmotion]) << endl;

        //follow up jokes
        if (rand() % 2 == 0) {
            string follow = randomFrom(followUps);
            cout << "Bot: " << follow << endl;
            lastBotQuestion = follow;
        } else {
            lastBotQuestion = "";
        }

        if (rand() % 5 == 0)
            cout << "Bot: " << randomFrom(jokes) << endl;

        if (abs(emotionalLoad) >= 4) {
            cout << "Bot: It feels like things have been weighing on you lately." << endl;
            emotionalLoad = 0;
        }
    }

    return 0;
}
