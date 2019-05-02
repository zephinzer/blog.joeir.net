---
layout: post
title: "Model View Controller Design Pattern"
published: true
---

> NOTE: This was a post migrated from my previous Ghost blog. If links don't work and you would really like to see it, it's probably posted somewhere in this blog.

This post addresses the Model-View-Controller (MVC) software design pattern in the context of a modern web application practicing proper software engineering techniques. 

## In A Nutshell

In a nutshell, MVC is a platform independent way of modelling the structure of an application that can be applied to data driven services involving Create/Read/Update/Delete (CRUD) operations. The MVC design pattern can be applied at each of the different levels of any given data driven software.

## The Model

On a modern web backend, Model components are involved in the retrieval and storage of data, communicating with the database directly. Using an Object-Oriented-Programming (OOP) design paradigm, these components are most often written as object classes which encapsulate methods related to a particular entity type that connect to a database and sends queries to store or retrieve data before passing them onto the Controller components.

## The Controller

The Controller components receive data from both the Model and View components. From the Model components, it receives data retrieved from the database. The Controllers then manipulate the data and change its form to one that is friendly to humans. For example, an options page may contain 8 checkboxes which users can toggle. These options can be represented as a byte in the interest of conserving space since a byte is composed of 8 1s and 0s. On data retrieval, the Controller converts a byte via bitmasking to its representative options which a user sees; From the View components, the Controller component receives 8 'true'/'false' options and converts it back into a byte before sending the data to the Model component which stores it in the database.

## The View

The View component receives and sends data from/to the Controller components and displays them accordingly. For example, a Controller can return a table of values. The View can display it as a table, or it can also display it as a graph depending on the requirements of the software. In the context of a web application, this tends to be the web interface which users interact with. The View component is also the receiver of data input by the user which may take the form of requests to update the data (which is sent to the Controller components) or even human interaction data such as clicking on a button that toggles the display of a dialog box.

## The Benefits Of MVC

In the context of a modern web app, the MVC design pattern allows for clear separation of data persistence (retrieval/storage), data manipulation, and data presentation. This manifests itself as a backend application (Model and Controller) and a frontend interface (View), connected by an Application Programming Interface (API). The View accesses the Controller via the API and requests/sends data through it to effect changes in the data model of the application as a whole. 

The benefits of this separation is the ability to design components independently of one another. For example when an interface overhaul is required, the backend application server can remain in tact if no additional functionality is required.

Another benefit is being able to do development work in stages, for example when data needs to be extended. The database schemas can be updated together with the Model components first without affecting the Controllers and Views, which can be updated later once the feature has been tested and verified to work as expected.

## When To Apply MVC

The MVC design pattern applies only when architecting systems where data is persisted and where there exists a need to display that data in a form possibly different from how it is stored. Beyond a design pattern, the MVC can be seen as a way of thinking about data. Traditionally, MVC describes a way of designing an application, but it can also be scaled up/down depending on necessity.

For example, in the design of a web platform, one might create a backend server and frontend webview with the former containing Model and Controller components and the latter consisting of Controller and (mostly) View components. Scaling it down to a more micro level, MVC can also be applied at just the frontend, thinking of functions calling the API as the Model components, functions that pass and manipulate the retrieved data to variables storing the retrieved data as Controllers and the HTML as the View. On the backend, one can think of the objects which retrieve data as the Model, the functions which manipulate the data as the Controller and the router which responds to the HTTP request as the View. 

## Conclusion

MVC is a widely used design pattern considering we are in the age where data is king and data creation, display, updating and removal (alternatively, CRUD operations - or as I call it, data logistics) is an essential part of any useful application. MVC allows the user to mentally segregate your software into components by how they interact with the data, and enables one to apply clear separation of concerns to the various layers of one's application.

Hope this helped someone out there (possibly some CS student forced into a software engineering module) have a clearer idea of what MVC is all about! Till the next time-
