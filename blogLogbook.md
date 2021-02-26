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

 