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

