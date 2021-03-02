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


