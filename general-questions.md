Time remaining
1:39:39
Companion Studios - Web Role Quiz
1. Foundational questions

For the foundational questions you can add example code snippets, but this is not necessary, as it's more about the concepts rather than implementation.

1.1: Foundation - Why would you want to create a deep clone of a JS data object, and how can you do it?

When you need to make a completely independent copy of an object, including all nested objects or arrays.

There are several methods:
- Using structuredClone() (modern and recommended): const clone = structuredClone(originalObject);
- Using JSON serialization (simple but limited): const clone = JSON.parse(JSON.stringify(originalObject));
1.2: Foundation - What is cross-site scripting, how would you prevent it, and how is it relevant to dangerouslySetInnerHTML

Cross-Site Scripting (XSS) is a security vulnerability that occurs when malicious scripts are injected into trusted web pages, allowing attackers to execute arbitrary code in the browser of other users. It can lead to data theft, session hijacking, or defacement of the site. 

To prevent XSS, developers should always sanitize and escape user input, use frameworks that automatically encode outputs, implement Content Security Policy (CSP), and avoid inserting raw HTML directly into the DOM. 
1.3: Foundation - Give two examples of when you would use Higher Order Components (HOCs) in React.

When you need to reuse logic or behavior across multiple components without duplicating code.One common example is authentication or authorization, where an HOC wraps protected components and checks if a user is logged in before rendering them (e.g., withAuthProtection(Component)). Another example is data fetching or state management, such as creating an HOC like withUserData(Component) that retrieves user information from an API and passes it as props to the wrapped component.
1.4: Foundation - Why does const a = { b: 1 }; a.b = 2 //result: a = { b: 2} not throw an error?

This code does not throw an error because the const keyword in JavaScript prevents reassignment of the variable reference, not mutation of the object’s contents.
1.5: Foundation - How does the Javascript event loop handle asynchronous operations

JavaScript event loop — which is single-threaded — to handle asynchronous operations efficiently without blocking the main thread. When asynchronous tasks such as API calls, timers, or event listeners are triggered, they are delegated to the Web APIs (or Node.js APIs) and continue running in the background. Once these tasks are completed, their callbacks are placed into the task queue (or callback queue).
1.6: Foundation - Explain a closure and how you would want to use one

A closure in JavaScript is a function that “remembers” the variables from its lexical scope, even after that scope has finished executing. Closures are useful when you want to encapsulate private data or maintain state between function calls without using global variables.
1.7: Foundation - Give two example situations of when you would use a `ref` in React?

One common situation is when working with imperative actions, such as focusing an input field automatically after rendering or validating forms (inputRef.current.focus()). Another example is when integrating third-party libraries that require direct DOM manipulation, such as initializing a custom chart, map, or animation library that does not fit into React’s declarative model.
2.1 Coding

Write a Javascript/Typescript function to find the maximum number in an array, returning the value and the index of the number in the original array ({index: number, value: number}), using functional programming where possible.

type MaxResult = { index: number; value: number };

function findMax(arr: number[]): MaxResult {
  if (arr.length === 0) throw new Error("Array cannot be empty");

  return arr.reduce<MaxResult>(
    (max, currentValue, currentIndex) =>
      currentValue > max.value
        ? { index: currentIndex, value: currentValue }
        : max,
    { index: 0, value: arr[0] }
  );
}
2.2 Coding

Write a Typescript function that takes in two string arrays and returns an array with all unique items from the two arrays, and sorted alphabetically.

mergeAndSort(['D','C','B'], ['D','B','A','E']) // Returns ['A','B','C','D','E']

function mergeAndSort(arr1: string[], arr2: string[]): string[] {
  return Array.from(new Set([...arr1, ...arr2])).sort();
}
3. Modern React and Typescript

Convert this button component written in Javascript to a strongly typed React Functional component in Typescript following the latest industry standards for large-scale maintainable projects.

import React, { Component } from 'react';
import PropTypes from 'prop-types';

class Button extends Component {
  render() {
      const { onClick, variation, text } = this.props;
      const buttonClass = variation === 'primary' ? 'primary-button' : 'secondary-button';

      return (
          <button className={buttonClass} onClick={onClick}>
               {text}
          </button>
      );
  }
}

Button.defaultProps = {
   variation: 'primary'
};

Button.propTypes = {
   onClick: PropTypes.func.isRequired,
   variation: PropTypes.oneOf(['primary', 'secondary']),
   text: PropTypes.string.isRequired
};

export default Button;

-------------------------------------------------------------------------------------------------------


import React from 'react';

type ButtonVariation = 'primary' | 'secondary';

interface ButtonProps {
  onClick: React.MouseEventHandler<HTMLButtonElement>;
  variation?: ButtonVariation;
  text: string;
}

export const Button: React.FC<ButtonProps> = ({
  onClick,
  variation = 'primary',
  text,
}) => {
  const buttonClass =
    variation === 'primary' ? 'primary-button' : 'secondary-button';

  return (
    <button className={buttonClass} onClick={onClick}>
      {text}
    </button>
  );
};
4.1 React - Write a custom React hook (using Typescript) and other relevant code that allows the “user-jwt” cookie to be set once and then fetched anywhere in the application. The hook definition should be:

type UseCookiesHook = (cookieName: string) => [userJwt: string, setUserJwt: (jwt: string) => void]

To get and set the cookies in the browser from within the hook, you can assume the following helper functions to be already implemented and available:

const getCookieByName = (name: string): string | null => { return null }
const setCookieByName = (name: string, value: string) => {}
 

Please include a way to make an instance of this hook available to all parts of the application using a Context provider called useUserJwtContext, that has an instance of the useCookies hook instantiated with the "user-jwt" cookie name.

 
 

import React, { createContext, useContext, useState, useEffect, ReactNode } from 'react';

// ---------- Helper functions (assumed to exist) ----------
const getCookieByName = (name: string): string | null => {
  return null;
};

const setCookieByName = (name: string, value: string): void => {};

// ---------- Hook Definition ----------
type UseCookiesHook = (cookieName: string) => [userJwt: string, setUserJwt: (jwt: string) => void];

export const useCookies: UseCookiesHook = (cookieName) => {
  const [userJwt, setUserJwtState] = useState<string>(() => getCookieByName(cookieName) || '');

  const setUserJwt = (jwt: string) => {
    setUserJwtState(jwt);
    setCookieByName(cookieName, jwt);
  };

  useEffect(() => {
    const storedJwt = getCookieByName(cookieName);
    if (storedJwt && storedJwt !== userJwt) {
      setUserJwtState(storedJwt);
    }
  }, [cookieName]);

  return [userJwt, setUserJwt];
};

// ---------- Context Implementation ----------
interface UserJwtContextType {
  userJwt: string;
  setUserJwt: (jwt: string) => void;
}

const UserJwtContext = createContext<UserJwtContextType | undefined>(undefined);

export const UserJwtProvider: React.FC<{ children: ReactNode }> = ({ children }) => {
  const [userJwt, setUserJwt] = useCookies('user-jwt');

  return (
    <UserJwtContext.Provider value={{ userJwt, setUserJwt }}>
      {children}
    </UserJwtContext.Provider>
  );
};

// ---------- Context Consumer Hook ----------
export const useUserJwtContext = (): UserJwtContextType => {
  const context = useContext(UserJwtContext);
  if (!context) {
    throw new Error('useUserJwtContext must be used within a UserJwtProvider');
  }
  return context;
};

4.2 React - What is your preferred way to manage state in a large React application and why?

My preferred approach is to use a combination of React Query (TanStack Query) for server state management and Redux Toolkit for client-side.
Answered 12 of 12 (100%)
Powered by FlexiQuiz.
