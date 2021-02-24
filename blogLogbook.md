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
src/context/BlogContext.js

Tworzymy kontext
const BlogContext = React.createContext();

eksporujemy stała Blog Provider

export const BlogProvider = ({ children }) => {
  return <BlogContext.Provider>{children}</BlogContext.Provider>;
};

# Moving data with context
useContext(BlogContext);

# rendering a list of posts
w BLogContext dodajemy

 const blogPosts = [{ title: 'Blog Post #1' }, { title: 'Blog Post #2' }];

 <BlogContext.Provider value={blogPosts}>

w IndexScreen  dodajemy

importujemy FlatList 

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

tworzymy adBlogPost =() =>
setBlogPosts([...blog posts {title: #${blogPosts.length +2}}])

doadjemy do blofcontext
value

# IT WORKS

IndexScreen.js
import Button from 'react-native'

destrukturyzaacja 2 zmiennych data, addBlogPost   z Contextu
const { data, addBlogPost } = useContext(BlogContext);

Tworzymy Button
  <Button title="Add Post" onPress={addBlogPost} />

  <FlatList data={data}