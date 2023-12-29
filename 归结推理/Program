//
//  main.cpp
//  归结推理
//
//  Created by zhangyc on 2023/12/29.
//

#include <iostream>
#include <string>
#include <vector>
#include <fstream>
#include <cctype>
#include <unordered_map>
#include <list>
#include <algorithm>

using namespace std;

class query
{
private:
    string sent;
    string predicate;
    vector<string> arguments;

public:
    query(string _sent)
    {
        if(_sent[0] == '!')
        {
            _sent.erase(0, 1);
        }
        else
        {
            _sent.insert(0, "!");

        }
        sent = _sent;

        string x;
        vector<string> arg;

        for(int i = 0; i < sent.length(); i++)
        {
            if(sent[i] != '(' && sent[i] != ')' && sent[i] != ',' && sent[i] != ' ')
            {
                x += sent[i];
            }
            else
            {
                if(sent[i] == '(')
                {
                    predicate = x;
                    x.clear();
                }
                if(sent[i] == ')')
                {
                    arg.push_back(x);
                    arguments = arg;
                    x.clear();
                }
                if(sent[i] == ',')
                {
                    arg.push_back(x);
                    x.clear();
                }
            }
        }
    }

    int argSize() const { return arguments.size(); }
    string arg(int i) const { return arguments[i]; }
    string pred() const { return predicate; }
    string line() const { return sent; }
};

class sentence
{
private:
    //literal - 1 = predicate, 2 = argument
    string sent;
    vector<string> predicates;
    vector<vector<string>> arguments;

public:
sentence(string _sent)
{
    sent = _sent;

    string x;
    int count = 0;
    int yes = 0;
    int size = 0;
    vector<string> pred;
    vector<vector<string>> arg;

    for(int i = 0; i < sent.length(); i++)
    {
        if(sent[i] == '&' || sent[i] == '>')
        {
            yes = 1;
        }

        if(sent[i] == '(')
        {
            size += 1;
        }
    }

    arg.resize(size);

    for(int i = 0; i < sent.length(); i++)
    {
        if(sent[i] != '(' && sent[i] != ')' && sent[i] != ',' && sent[i] != ' ' && sent[i] != '&' && sent[i] != '|')
        {
            x += sent[i];
        }
        else
        {
            if(yes == 0)
            {
                if(sent[i] == '(')
                {
                    pred.push_back(x);
                    x.clear();
                }
                if(sent[i] == ')')
                {
                    arg[count].push_back(x);
                    x.clear();
                    count += 1;
                }
                if(sent[i] == ',')
                {
                    arg[count].push_back(x);
                    x.clear();
                }
            }
            else if(yes == 1)
            {
                if(x == "->")
                {
                    yes = 0;
                    x.clear();
                }
                if(sent[i] == '(')
                {
                    x.insert(0, "!");
                    pred.push_back(x);
                    x.clear();
                }
                if(sent[i] == ')')
                {
                    arg[count].push_back(x);
                    x.clear();
                    count += 1;
                }
                if(sent[i] == ',')
                {
                    arg[count].push_back(x);
                    x.clear();
                }
            }
        }
    }
    predicates = pred;
    arguments = arg;
}

sentence(string p, vector<string> a)
{
    vector<string> pred;
    vector<vector<string>> arg;
    pred.push_back(p);
    predicates = pred;
    arg.resize(1);
    string s = p;
    s += "(";
    for(int i = 0; i < a.size(); i++)
    {
        if(i == a.size() - 1)
        {
            s = s + a[i] + ")";
        }
        else
        {
            s = s + a[i] + ",";
        }
        arg[0].push_back(a[i]);
    }
    sent = s;
    arguments = arg;
}

sentence(vector<string> pred, vector<vector<string>> arg)
{
    predicates = pred;
    arguments = arg;
    string x;

    for(int i = 0; i < pred.size(); i++)
    {
        x = x + pred[i] + "(";
        for(int j = 0; j < arg[i].size(); j++)
        {
            if(j == arg[i].size() - 1)
            {
                x = x + arg[i][j] + ")";
            }
            else
            {
                x = x + arg[i][j] + ",";
            }
        }
        if(i != pred.size() - 1)
        {
            x = x + " | ";
        }
    }

    sent = x;
}

int predSize() const { return predicates.size(); }
int argSize(int i) const {return arguments[i].size(); }
string pred(int i) const { return predicates[i]; }
string arg(int i, int j) const { return arguments[i][j]; }
    
vector<string> args(int i) { return arguments[i]; }
void setPred(int i, string x) { predicates[i] = x; }
void setArg(int i, int j, string x) { arguments[i][j] = x; }
string line() const {return sent; }
    
bool predEmpty() {
    if(predicates.empty())
    {
        return true ;
    }
    else
    {
        return false;
    }
}

};

vector<query> queries;
vector<sentence> kb;
int num = 1;

void getInput()
{
    string sent;
    int num = 0;
    
    getline(cin, sent);
    while(sent[sent.length() - 1] == '\n' || sent[sent.length() - 1] == '\r')
    {
        sent.erase(sent.length() - 1);
    }
    num = stoi(sent);
    //Add all Queries to Query Library
    for(int i = 0; i < num; i++)
    {
        getline(cin, sent);
        while(sent[sent.length() - 1] == '\n' || sent[sent.length() - 1] == '\r')
        {
            sent.erase(sent.length() - 1);
        }
        queries.push_back(query(sent));
    }
    
    getline(cin, sent);
    while(sent[sent.length() - 1] == '\n' || sent[sent.length() - 1] == '\r')
    {
        sent.erase(sent.length() - 1);
    }
    num = stoi(sent);
    while(sent[sent.length() - 1] == '\n' || sent[sent.length() - 1] == '\r')
    {
        sent.erase(sent.length() - 1);
    }
    //Add all Sentences to KB
    for(int i = 0; i < num; i++)
    {
        getline(cin, sent);
        while(sent[sent.length() - 1] == '\n' || sent[sent.length() - 1] == '\r')
        {
            sent.erase(sent.length() - 1);
        }
        kb.push_back(sentence(sent));
    }
    return;
}

void output(vector<string> solns)
{
    for(int i = 0; i < solns.size(); i++)
    {
        cout << solns[i];
        if(i != solns.size() -1)
        {
            cout << "\n";
        }
    }

    return;
}

vector<sentence> formatKB(vector<sentence> k)
{
    //Get rid of double negation
    string x;
    for(int i = 0; i < k.size(); i++)
    {
        for(int j = 0; j < k[i].predSize(); j++)
        {
            if(k[i].pred(j)[0] == '!' && k[i].pred(j)[1] == '!')
            {
                x = k[i].pred(j);
                x.erase(0, 2);
                k[i].setPred(j, x);
            }
        }
    }

    return k;
}

bool inKB(vector<sentence> knowbase, sentence in)
{

    int count = 0;
    for(int i = 0; i < knowbase.size(); i++)
    {
        if(knowbase[i].predSize() == in.predSize())
        {
            count = 0;
            for(int j = 0; j < in.predSize(); j++)
            {
                for(int k = 0; k < knowbase[i].predSize(); k++)
                {
                    if(in.pred(j) == knowbase[i].pred(k))
                    {
                        for(int m = 0; m < in.argSize(j); m++)
                        {
                            //Check if the arguments are not equal -> not in KB
                            if((!islower(knowbase[i].arg(k, m)[0]) && !islower(in.arg(j, m)[0]) && in.arg(j, m) != knowbase[i].arg(k, m)) || (!islower(knowbase[i].arg(k, m)[0]) && islower(in.arg(j, m)[0])) || (islower(knowbase[i].arg(k, m)[0]) && !islower(in.arg(j, m)[0])) || (islower(knowbase[i].arg(k, m)[0]) && islower(in.arg(j, m)[0]) && in.arg(j, m)[0] != knowbase[i].arg(k, m)[0]))
                            {
                                break;
                            }
                            else if(m == in.argSize(j) - 1) //same
                            {
                                count += 1;
                            }
                        }
                    }
                }
            }
        }
        if(count == in.predSize())
        {
            return true;
        }
    }
    return false;
}

sentence unify(sentence k1, sentence k2, int index1, int index2)
{
    vector<string> preds;
    vector<vector<string>> args;
    unordered_map<string, string> vars1;
    unordered_map<string, string> vars2;
    string temp;
    int count = k1.predSize() + k2.predSize();
    for(int j = 0; j < k1.argSize(index1); j++)
    {
        if((islower(k1.arg(index1, j)[0]) && !islower(k2.arg(index2, j)[0])))
        {
            if(vars1.find(k1.arg(index1, j)) == vars1.end()) //Not in vars1
            {
                vars1[k1.arg(index1, j)] = k2.arg(index2, j); //k1(variable) = k2(constant)
            }
            else if(vars1[k1.arg(index1, j)] != k2.arg(index2, j) && !islower(vars1[k1.arg(index1, j)][0]))//Unify A(x,x) and A(Alice,Max)
            {
                preds.push_back("invalid");
                args.resize(1);
                return sentence(preds, args);
            }
            else if(vars1[k1.arg(index1, j)] != k2.arg(index2, j) && islower(vars1[k1.arg(index1, j)][0]))
            {
                temp = vars1[k1.arg(index1, j)];
                vars1[k1.arg(index1, j)] = k2.arg(index2, j);

                for(pair<string, string> element: vars2)
                {
                    if(element.second == temp)
                    {
                        vars2[element.first] = k2.arg(index2, j);
                    }
                }
            }
        }
        else if(islower(k2.arg(index2, j)[0]) && !islower(k1.arg(index1, j)[0]))
        {
            if(vars2.find(k2.arg(index2, j)) == vars2.end())//If the variable is not already in vars2
            {
                vars2[k2.arg(index2, j)] = k1.arg(index1, j); //Assign k2(variable) = k1(constant)
            }
            else if(vars2[k2.arg(index2, j)] != k1.arg(index1, j) && !islower(vars2[k2.arg(index2, j)][0]))
            {
                preds.push_back("invalid");
                args.resize(1);
                return sentence(preds, args);
            }
            else if(vars2[k2.arg(index2, j)] != k1.arg(index1, j) && islower(vars2[k2.arg(index2, j)][0]))
            {
                temp = vars2[k2.arg(index2, j)];
                vars2[k2.arg(index2, j)] = k1.arg(index1, j);

                for(pair<string, string> element: vars1)
                {
                    if(element.second == temp)
                    {
                        vars1[element.first] = k1.arg(index1, j);
                    }
                }
            }
        }
        else if((k1.arg(index1, j) != k2.arg(index2, j)) && (!islower(k1.arg(index1, j)[0]) && !islower(k2.arg(index2, j)[0])))
        {
            //k1 and k2 have constant arguments that are not equal
            preds.push_back("invalid");
            args.resize(1);
            return sentence(preds, args);
        }
        else if((islower(k1.arg(index1, j)[0]) && islower(k2.arg(index2, j)[0]) && k1.arg(index1, j)[0] != k2.arg(index2, j)[0]))
        {
            if(vars1.find(k1.arg(index1, j)) == vars1.end() && vars2.find(k2.arg(index2, j)) == vars2.end())
            {
                temp = k2.arg(index2, j) + to_string(num);
                num += 1;
                vars2[k2.arg(index2, j)] = temp;
                vars1[k1.arg(index1, j)] = temp;
            }
            else if(vars1.find(k1.arg(index1, j)) == vars1.end() && vars2.find(k2.arg(index2, j)) != vars2.end())
            {
                vars1[k1.arg(index1, j)] = vars2[k2.arg(index2, j)];
            }
            else if(vars1.find(k1.arg(index1, j)) != vars1.end() && vars2.find(k2.arg(index2, j)) == vars2.end())
            {
                vars2[k2.arg(index2, j)] = vars1[k1.arg(index1, j)];
            }
            else if(vars1.find(k1.arg(index1, j)) != vars1.end() && vars2.find(k2.arg(index2, j)) != vars2.end())
            {
                preds.push_back("invalid");
                args.resize(1);
                return sentence(preds, args);
            }
        }

        if(j == k1.argSize(index1) - 1) //Both empty
        {
            count = count - 2; //Resize
        }
    }

    args.resize(count);
    count = 0;
    //Add the items in k1 to the new sentence & unify
    for(int i = 0; i < k1.predSize(); i++)
    {
        if(i != index1)
        {
            preds.push_back(k1.pred(i));
            for(int j = 0; j < k1.argSize(i); j++)
            {
                if(vars1.find(k1.arg(i, j)) == vars1.end()) //If the variable from 2nd sentence isn't in the 1st sentence
                {
                    args[count].push_back(k1.arg(i,j)); //add from k1
                }
                else
                {
                    args[count].push_back(vars1.find(k1.arg(i, j))->second); //add from vars1
                }
            }
            count++;
        }
    }

    //Add the items in k2 to the new sentence & unify
    for(int i = 0; i < k2.predSize(); i++)
    {
        if(i != index2)
        {
            preds.push_back(k2.pred(i));
            for(int j = 0; j < k2.argSize(i); j++)
            {
                if(vars2.find(k2.arg(i,j)) == vars2.end())
                {
                    args[count].push_back(k2.arg(i,j));
                }
                else
                {
                    args[count].push_back(vars2.find(k2.arg(i, j))->second);
                }
            }
            count++;
        }
    }

    //Delete duplicates
    for(int w = 0; w < preds.size(); w++)
    {
        for(int i = w + 1; i < preds.size(); i++)
        {
            if(preds[w] == preds[i])
            {
                for(int j = 0; j < args[i].size(); j++)
                {
                    if(args[w][j] != args[i][j])
                    {
                        break;
                    }
                    else if(j == args[i].size() - 1)
                    {
                        preds.erase(preds.begin() + i);
                        args.erase(args.begin() + i);
                    }
                }
            }
        }
    }

    return sentence(preds, args);
}

string resolution(query q)
{
    list<int> visited;
    vector<sentence> kbase = kb;
    kbase.push_back(sentence(q.line()));
    kbase = formatKB(kbase);

    bool finished = false;
    bool found = false;
    int kbsize = kbase.size();
    string one, two;
    int pointer = 0;
    int number = kbase.size();
    int kbs = kbsize - 1;
    int noSolution = 0;
    int count = 0;

    while(!finished)
    {
        for(int x = 0; x < kbsize; x++)
        {
            if(kbase[x].predSize() == 1 && find(visited.begin(), visited.end(), x) == visited.end())
            {
                pointer = x;
                visited.push_back(x);
                break;
            }
        }
        int i = 0;
        count = 0;
        found = false;
        while(i < kbsize)
        {
            NEXT:
            if(found)
            {
                i += 1;
            }
            found = false;
            for(int j = 0; j < kbase[i].predSize(); j++)
            {
                two = kbase[i].pred(j);
                for(int k = 0; k < kbase[pointer].predSize(); k++)
                {
                    if(i != pointer)
                    {
                        one = kbase[pointer].pred(k);
                        if((one[0] == '!' && two[0] != '!' && one.substr(1, one.length()) == two) || (one[0] != '!' && two[0] == '!' && two.substr(1, two.length()) == one))
                        {
                            sentence u = unify(kbase[pointer], kbase[i], k, j);
                            if(u.predEmpty())
                            {
                                finished = true;
                                return "TRUE";
                            }
                            else if(u.pred(0) != "invalid" && !inKB(kbase, u))
                            {
                                kbase.push_back(u);
                                found = true;
                                count++;
                                number++;
                                goto NEXT;
                            }
                        }
                    }

                }
            }
            i += 1;

            if(count > 0 && i == kbsize)
            {
                kbs += 1;
                pointer = kbs;
                count--;
                i = 0;
                found = false;
            }
        }
        kbsize = kbase.size();
        kbs = kbsize - 1;
        if(noSolution == kbsize)
        {
            noSolution -= 5;
        }
        else if(noSolution == kbsize - 5)
        {
            finished = true;
        }
        else
        {
            noSolution = kbsize;
        }
    }

    return "FALSE";
}

void completeResolution()
{
    vector<string> solutions;
    for(int i = 0; i < queries.size(); i++)
    {
        solutions.push_back(resolution(queries[i]));
    }
    output(solutions);
    return;
}

int main()
{
    getInput();
    completeResolution();
    cout << endl;
    return 0;
}
