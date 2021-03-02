# 24.02.2021
# AppOverView
Index
Show
Create
Edit

# Issues with data
npm install react-navigation

# react navigation stack navigator:
- IndexScreen:
BlogList
- ShowScreen
- CreateScreen
- EditScreen

blogposts:[]

Blog Post Provider

# Initial setup

# Wrapping The navigator
Przypisanie eksportu do zmiennej App

Stworzenie export default () =>


# Introduction to context

# Adding context
# Instrucja do tworzenia kontekstu

1. src/context/BlogContext.js

2. Tworzymy kontext
const BlogContext = React.createContext();
3. Tworzymy funcję która przyjmuje children w propsach.
i zwra elemrnt otagowany <BlogContext.Provider>:

export const BlogProvider = ({ children }) => {
  return <BlogContext.Provider>{children}</BlogContext.Provider>;
};

4. eksporujemy stała Blog Provider

5. importujemy w App.js

import { BlogProvider}


6. następinie w App.js otagowujemy App

export default () => {
  return (
    <Provider>
      <App />
    </Provider>
  );
};

# Moving data with context
7. aby przekazać informację importujemy w IndexScreen.js BlogContext  oraz useContext z react

8. destukturyzujemy value

const value = useContext(BlogContext)




9. Dzięki temu możemy dowolny element przekazywać dowolnemu komponentowi za pomoca wartości i kontekstu

# rendering a list of posts
w BLogContext tworzymy liste postów

 const blogPosts = [{ title: 'Blog Post #1' }, { title: 'Blog Post #2' }];

 <BlogContext.Provider value={blogPosts}>

w IndexScreen  dodajemy

importujemy FlatList bo text tecgo nie wyrebderue

pamiętamy o tym:
const blogPosts = useContext(BlogContext);



 <FlatList
        data={blogPosts}
        keyExtractor={blogPost => blogPost.title}
        renderItem={({ item }) => {
          return <Text>{item.title}</Text>;
        }}
      />


# Adding State in Context
w BLogContext dodajemy:
import {usestate}

tworzymy hooka statea

tworzymy addBlogPost =() =>
setBlogPosts([...blog posts {title: #${'blogPosts.length +1}' }])

doadjemy do blogContext aby przekazać
value= {{data :blogPosts, addBlogPost}}

# IT WORKS
 
IndexScreen.js
import Button from 'react-native'

destrukturyzaacja 2 zmiennych data, addBlogPost   z Contextu


const { data, addBlogPost } = useContext(BlogContext);

Tworzymy Button
  <Button title="Add Post" onPress={addBlogPost} />

  <FlatList data={data}

#  Opportunity for Improvement-Szansa na poprawę
w BLogContext:
Zamiast tworzyć oddzielną funkcję dla każej operacji :
const editBlogPost
const deleteBlogPost

Urzyjemu UseReducer zamias UseState 
import React, { useReducer } from 'react';

# Updating with UseReducer
to powtórzyć!!!
Tworzenie dispatchera i 
reduktora 

# Automatic context creator
src/context/createDataContext.js


      <Context.Provider value={{ state, ...boundActions }}>
        {children}
      </Context.Provider>
    );
  };

  return { Context, Provider };
};

# A Bit of styling
Z IndexScreen.js usówamy
<Text> IndexScreen
 

 Iportujemy expo icons
 trash

 # Deleting Posts
 dodaliśmy w IndexScreen.js import TouchableOpacity

 <Text style={styles.title}>
              <Feather style={styles.icon} name="trash" />	                {item.title} - {item.id}
              </Text>
              <TouchableOpacity onPress={() => console.log(item.id)}>
                <Feather style={styles.icon} name="trash" />

 wygenerowaliśmy w BlogContext.js losowe id

 id: Math.floor(Math.random() * 99999),

# Updating the reducer
BlogContext.js

do reducera dodaliśmy
  case 'delete_blogpost':
      return state.filter(blogPost => blogPost.id !== action.payload)

Stworzyliśmy nową funkcję dispatchująca
!! przesyłamy id
const deleteBlogPost = dispatch => {
  return id => {
    dispatch({ type: 'delete_blogpost', payload: id });
  };
};

Przesłaliśmu  naszego dispatchera w kontekście
blogReducer,
  { addBlogPost },	  { addBlogPost, deleteBlogPost },
  []

  W IndexScreen.js 
dodaliśmy deleteBlogPost do useState

const { state, addBlogPost, deleteBlogPost } = useContext(Context);


oraz 
onPress

<TouchableOpacity onPress={() => deleteBlogPost(item.id)}>

# Navigation on Tap
src/screens/ShowScreen.js
na poczatku mock

const ShowScreen = () => {
  return (
    <View>
      <Text>Show Screen</Text>
    </View>
  );
};

w App.js  importujemy i dodajemy do navwigator

const navigator = createStackNavigator(


w IndexScreen.js  przekazujemy 

const IndexScreen = ({ navigation })


dodajemy przycisk TouchableOpacity
      onPress={() => navigation.navigate('Show', { id: item.id })}	 

# Retrieving Single Posts- Pobieranie pojedynczych postów
chcemy zrobić tak żeby w ShowScreen pojawała się nazwa postu

w ShowScreen.js
importujemy kontext

import React, { useContext } from 'react';

import { Context } from '../context/BlogContext';

przesyłamy navigation do componentu
const ShowScreen = ({ navigation }) =>


destrukturyzujemy state z kontekstu
  const { state } = useContext(Context)

w zmiennej blogPost pobieramy parametry dla dnaego posta 

  const blogPost = state.find(
    blogPost => blogPost.id === navigation.getParam('id')
  );

wyświetlamy tytuł 
<Text>{blogPost.title}</Text>

# Adding a Creation Screen

src/screens/CreateScreen.js

kopiujemy ShowScrena do Create Screena

zmieniamy na Create Screan 

importujemy go w App.js

dodajemy do navigatora


# Header Navigation
Tworzymy przycisk który nawiguje do Create

IndexScreen.js

Przestarzałe:

IndexScreen.navigationOptions = ({ navigation }) => {
  return {
    headerRight: (
      <TouchableOpacity onPress={() => navigation.navigate('Create')}>
        <Feather name="plus" size={30} />
      </TouchableOpacity>
    )
  };
};

 obecnie headerRight: () => (

# Displaying a Form

dodanie 2 inputtext
dodadnie 2 useState
dodanie buttona
stylizacja

# Saving a New Post

BlogContext.js
zmieniamy funcję dispatchera
addBlogPost 


CreateScreen.js 
dodajemy 
const { addBlogPost } = useContext(Context);

przyspisujemy do buttona funkcje dispatchująca

 <Button
        title="Add Blog Post"
        onPress={() => addBlogPost(title, content)}
      />


# Navigation on Save

CreateScreen.js
- dodajemy funkcje navigation do buttona

<Button
        title="Add Blog Post"
        onPress={() => {
          addBlogPost(title, content, () => {
            navigation.navigate('Index');
          });
        }}
        
BlogContext.js 
- dodajemy callback

const addBlogPost = dispatch => {
  return (title, content, callback) => {
    dispatch({ type: 'add_blogpost', payload: { title, content } });
    callback();
  };
};

IndexScreen.js
- usówamy niepotrzebny button

const IndexScreen = ({ navigation }) => {
  const { state, deleteBlogPost } = useContext(Context);

 ## Deprecation in 'navigationOptions':
- 'headerRight: <SomeElement />' will be removed in a future version. Use 'headerRight: () => <SomeElement />' instead


# The Edit Icon Link
Chcemy dodać ikonę edycji
ShowScreen.js

importujemy TouchableOpacity oraz
import { EvilIcons } from '@expo/vector-icons';

dodajemy  aby wyśiwtlało content
 <Text>{blogPost.content}</Text>

Piszemy funkcje navigationOptions:
 ShowScreen.navigationOptions = ({ navigation }) => {
  return {
    headerRight: (
      <TouchableOpacity onPress={() => navigation.navigate('Edit')}>
        <EvilIcons name="pencil" size={35} />
      </TouchableOpacity>
    )
  };
};

# Communicating Info to Edit

- BlogContext.js
 dodajemy post testowy predefiniowany
  [{ title: 'TEST POST', content: 'TEST CONTENT', id: 1 }]



Tworzymy nowy ekran któremu przekazujemy parametr:
- blog/src/screens/EditScreen.js 

const EditScreen = ({ navigation }) => {
  return (
    <View>
      <Text>Edit Screen - {navigation.getParam('id')}</Text>
    </View>
  );
};
 
- App.js
importujemy nowy komponent ekranowy
import EditScreen from './src/screens/EditScreen';
dodajemy do nawigatora
 Create: CreateScreen,
    Edit: EditScreen




- ShowScreen.js 
uzupełniamy on Press
 onPress={() =>
          navigation.navigate('Edit', { id: navigation.getParam('id') })
        }


# Initializing State from Context

- EditScreen.js

imporyujemy
 { useState, useContext }
  TextInput 
  import { Context } from '../context/BlogContext';

  w komponencie:
  destrukturyzujemy
    const { state } = useContext(Context);

      const blogPost = state.find(
    blogPost => blogPost.id === navigation.getParam('id')
  );

  const [title, setTitle] = useState(blogPost.title);
  const [content, setContent] = useState(blogPost.content);


w widoku:
<Text>Edit Title:</Text>
      <TextInput value={title} onChangeText={newTitle => setTitle(newTitle)} />

# Extracting Form Logic

# Customizing OnSubmit
- blog/src/components/BlogPostForm.js 
tworzymy pusty komponent BlogPostForm
kopiujemy całe view z creatScreen
  <View>
      <Text style={styles.label}>Enter Title:</Text>
      <TextInput
        style={styles.input}
        value={title}
        onChangeText={text => setTitle(text)}
      />
      <Text style={styles.label}>Enter Content:</Text>
      <TextInput
        style={styles.input}
        value={content}
        onChangeText={text => setContent(text)}
      />
      <Button title="Save Blog Post" onPress={() => onSubmit(title, content)} />
    </View>

    oraz  styles


    oczyuwiście importujeemy TextInput oraz Button

kopiujemy zmienne stanu

  const [title, setTitle] = useState('');	
  const [content, setContent] = useState('');

- CreateScreen.js
importujemy BlogPOstForm


 return (
 <BlogPostForm
      onSubmit={(title, content) => {
        addBlogPost(title, content, () => navigation.navigate('Index'));
      }}


usówamy niepotrzebne importy
- EditScreen.js
importujemy BlogPOstForm

 return <BlogPostForm />;

 Całym statem zarzadza BLogPostForm




# Initial Form Values
- BlogPostForm.js 
Dodajemy initialValues do state

const BlogPostForm = ({ onSubmit, initialValues }) => {
  const [title, setTitle] = useState(initialValues.title);
  const [content, setContent] = useState(initialValues.content);

- EditScreen.js 
dodajemy propsy initialValues

<BlogPostForm
      initialValues={{ title: blogPost.title, content: blogPost.content }}
      onSubmit={(title, content) => {
        console.log(title, content);
      }}
    />




# Default Props
- BlogPostForm.js

BlogPostForm.defaultProps = {
  initialValues: {
    title: '',
    content: ''
  }
};
# Editing Action Function
# Editing in a Reducer 

- BlogContext.js
dodajemy do reducera
case 'edit_blogpost':
      return state.map(blogPost => {
        return blogPost.id === action.payload.id ? action.payload : blogPost;
      })

tworzymy funkcję dispatchującą
const editBlogPost = dispatch => {
  return (id, title, content) => {
    dispatch({
      type: 'edit_blogpost',
      payload: { id, title, content }
    });
  };
};

w eksporcie eksportujemy tą funkcję

- EditScreen.js
przypisujemy do zmiennych
 const id = navigation.getParam('id');
  const { state, editBlogPost } = useContext(Context);

  dzięki temu możemy skrócić funkcje
  const blogPost = state.find(blogPost => blogPost.id === id);


  w return 
  zamiast consoleloga wywołujemy funkcję dispatchująca

  editBlogPost(id, title, content);

# Navigating Backwards  
- BlogContext.js  do add i edit
     if (callback) {
      callback();
   }

- EditScreen.js 

 editBlogPost(id, title, content, () => navigation.pop());

 # Outside Data API
npmjs.com/package/json-server

# Issues with Servers + React Native
tworzę nowy folder jsonserver

npm install json-server ngrok

# JSON Server and Ngrok Setup
jsonserver/db.json


{
  "blogposts": [
    {
      "id": 1,
      "title": "API POST",
      "content": "content for my post"
    }
  ]
}

package.json

"scripts": {
    "db": "json-server -w db.json",
    "tunel": "ngrok http 3000"



## npm run db
## npm run tunnel


# npm install axios


- .gitignore

node_modules

- IndexScreen.js
import  useEffect

dodajemy getBlogPosts do destrukturyzacji z kontekstu

useEffect(() => {
    getBlogPosts();
  }, []);

# JSON Server REST Conventions
GET,POST,PUT,DELETE

# Making a Request

- blog/src/api/jsonServer.js

import axios from 'axios';

export default axios.create({
  baseURL: 'http://d3758ea270d6.ngrok.io'
});

- BlogContext.js

import jsonServer from '../api/jsonServer'

dodajemy do blogReducera 
case 'get_blogposts':
      return action.payload;

tworzymy nową funkcję dispatchującą

const getBlogPosts = dispatch => {
  return async () => {
    const response = await jsonServer.get('/blogposts');

    dispatch({ type: 'get_blogposts', payload: response.data });
  };
};

dodajemy getBlogPosts do contekstu

# Remote Fetch of Posts
IndexScreen.js
import React, { useContext, useEffect 

dodajemygetBlogPost 
 const { state, deleteBlogPost, getBlogPosts } = useContext(Context);

  useEffect(() => {
    getBlogPosts();
  }, []);