# About

All notes are my course notes based on learning through various courses:

**React & NextJS**

- `Professional React and Next.JS` - a course produced by Wesley at [ByteGrad.com](https://bytegrad.com).
- `The Ultimate React Course 2024` - a course by [Jonas Schmedtmann](https://www.udemy.com/course/the-ultimate-react-course/)

# Contents

## React

- [Components and Props](#) - ToDo
- [JSX](#) - ToDo
- [State Management](./react/state.md#state)
  - [URL](./react/state.md#url)
    - [Writing Hash](./react/state.md#write-hash)
    - [Reading Hash](./react/state.md#read-hash)
  - [useState()](#) - ToDo
  - [useReducer()](#) - ToDo
  - [useContext()](#) - ToDo
- [Handling Side Effects](#) - ToDo
- [useRef() for Elements](#) - ToDo
- [Server Actions](./react/server-actions.md)
  - [use server directive](./react/server-actions.md#use-server-directive)
  - [Security](./react/server-actions.md#security)
  - [Supported Arguments](./react/server-actions.md#supported-arguments)
  - [Forms](./react/server-actions.md#server-actions-in-forms)
- [Performance Optimisations](./react/optimisations.md#optimisations)
  - [Debouncing](./react/optimisations.md#debouncing)
  - [Memoisation](#) - ToDo

## React Packages

- [React Hot Toast](./react/react-packages/react-hot-toast.md#react-hot-toast)
  - [Positioning](./react/react-packages/react-hot-toast.md#toast-position)
  - [Triggers](./react/react-packages/react-hot-toast.md#trigger-toast)
  - [Toast IDs](./react/react-packages/react-hot-toast.md#toast-id)
- [React Router](./react/react-packages/react-router.md)
  - [Creating a Route - the old way](./react/react-packages/react-router-old.md)
  - [Creating a Route - the new way](./react/react-packages/react-router.md#creating-route)
  - [Link](./react/react-packages/react-router.md#linking-between-pages)
  - [NavLink](./react/react-packages/react-router.md#navlink)
  - [Using the URL](./react/react-packages/react-router.md#storing-state-in-the-url)
    - [Params](./react/react-packages/react-router.md#params)
    - [Query String](./react/react-packages/react-router.md#query-string)
  - [Programmatic Navigation](./react/react-packages/react-router.md#programatic-navigation)
    - [Imperative - useNavigate()](./react/react-packages/react-router.md#imperative-with-the-usenavigate-hook)
    - [Declarative - <Navigate />](./react/react-packages/react-router.md#declarative-with-the-navigate--component)
- [Redux](./react/react-packages/redux.md)

  - [Overview](./react/react-packages/redux.md#overview)
  - [When to use Redux](./react/react-packages/redux.md#when-should-we-use-redux)
  - [How it works](./react/react-packages/redux.md#how-does-redux-work)
  - [Installation](./react/react-packages/redux.md#installation)
  - [File structures](./react/react-packages/redux.md#file-structures)
  - [Redux Toolkit](./react/react-packages/redux.md#redux-toolkit)
    - [Creating a Slice](./react/react-packages/redux.md#create-a-slice)
    - [Creating a Store](./react/react-packages/redux.md#create-the-store-with-toolkit)
    - [Connecting store with App](./react/react-packages/redux.md#connecting-the-store-to-the-app)
    - [Typing state and store](./react/react-packages/redux.md#typing-state-and-store)
    - [Accessing state](./react/react-packages/redux.md#accessing-state)
  - [Oldschool Redux](./react/react-packages/redux.md#oldschool-redux)
    - [Initial State and Reducers](./react/react-packages/redux.md#initial-state--reducers)
    - [Using multiple reducers](./react/react-packages/redux.md#using-multiple-reducers)
    - [Creating a store](./react/react-packages/redux.md#create-a-store)
    - [Dispatch](./react/react-packages/redux.md#dispatch)
    - [Actions](./react/react-packages/redux.md#actions)

- [zustand](#) - ToDo

## NextJS

- [Installation and Setup](./nextjs/setup.md#installation-and-setup)
  - [Installation](./nextjs/setup.md#installation)
  - [Add Prettier for Tailwind and ESLint](./nextjs/setup.md#add-prettier-and-prettier-for-tailwind-and-eslint)
  - [Set project word wrapping and formatter](./nextjs/setup.md#enable-wordwrapping-and-set-default-formatter-in-vs-code)
- [Page Setup Basics](./nextjs/page-basics.md#page-setup-basics)
  - [Set page title and description](./nextjs/page-basics.md#set-page-title-and-description)
  - [Favicon](./nextjs/page-basics.md#favicon)
- [Route Management](./nextjs/route-management.md#route-management)
  - [Create a Route](./nextjs/route-management.md#create-a-route)
  - [Create a Dynamic Route](./nextjs/route-management.md#create-a-dynamic-route)
  - [Get the current path](./nextjs/route-management.md#get-the-current-pathname)
  - [Access the route](./nextjs/route-management.md#access-the-route)
  - [Access the params](./nextjs/route-management.md#access-the-params)
  - [Search Params](./nextjs/route-management.md#search-params)
- [Built-in NextJS Components](./nextjs/nextjs-components.md#built-in-nextjs-components)
  - [Link](./nextjs/nextjs-components.md#the-link-component)
  - [Image](./nextjs/nextjs-components.md#the-image-component)
- [Special Files](./nextjs/special-files.md#special-files)
  - [404 Not Found](./nextjs/special-files.md#404-not-found-component)
  - [Loading](./nextjs/special-files.md#loading-states)
  - [Error](./nextjs/special-files.md#error-page)
- [Security](./nextjs/security.md#security)
  - [Cross-origin Sites](./nextjs/security.md#cross-origin-sites)
    - [Images](./nextjs/security.md#for-images)
  - [Server Only Utilities](./nextjs/security.md#server-only-utilities)
- [Server Actions](./nextjs/server-actions-njs.md#server-actions-in-nextjs)
  - [UI Feedback](./nextjs/server-actions-njs.md#ui-feedback)
    - [Re-validation](./nextjs/server-actions-njs.md#re-validation)
    - [Fork Resets](./nextjs/server-actions-njs.md#form-resets)
    - [Action Status](./nextjs/server-actions-njs.md#status)
  - [Optimistic Rendering](./nextjs/server-actions-njs.md#optimistic-rendering)
  - [Error Handling](./nextjs/server-actions-njs.md#error-handling)
- [Caching](./nextjs/cache.md#caching)
  - [unstable_cache](./nextjs/cache.md#unstable_cache)
- [Middleware](./nextjs/middleware.md#middleware)
  - [Page Redirects](./nextjs/middleware.md#page-redirect)
- [Optimisations](./nextjs/optimisations.md#optimisations)
  - [Generate Static Params](./nextjs/optimisations.md#generate-static-params)

### Extra Features

- [Opengraph Images](./nextjs/extra-features.md#opengraph-images)

### Useful Patterns

- [Fetch Wrappers](./nextjs/fetch-wrapper#fetch-wrapper)
- [Data Wrappers](#) - Todo

# Other Packages

- [Framer Motion](./packages/framer-motion.md#framer-motion)
  - [Motion Elements](./packages/framer-motion.md/#create-a-motion-element)
  - [useScroll Hook](./packages/framer-motion.md/#framer-motion-usescroll-hook)
  - [useTransform Hook](./packages/framer-motion.md/#usetransform)
- [Prisma](./packages/prisma.md#prisma)
  - [Model Creation](./packages/prisma.md#create-model)
  - [Seeding](./packages/prisma.md/#seed-the-db)
  - [DB Client File](./packages/prisma.md/#create-db-client-file)
  - [Access Database](./packages/prisma.md/#access-db)
  - [Sort Results](./packages/prisma.md/#sort-results)
  - [Limit Resutls](./packages/prisma.md#limit-results)
  - [Skip Results](./packages/prisma.md#skip-results)
  - [Number of Results](./packages/prisma.md#number-of-results)
  - [Deployment Client Generation](./packages/prisma.md#post-install-client-generation-for-deployment)
- [Zod Validation](./packages/zod.md)
