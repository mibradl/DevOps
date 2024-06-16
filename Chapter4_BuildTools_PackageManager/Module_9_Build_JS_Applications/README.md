# Chapter4
This module we reviewed how to Run the JavaScript Application.

# Name: Running JavaScript Application

# Description: 

Build JavaScript applications

No special artifact type (Can be a zip or tar file)

NPM and Yarn (these are similar tools, in that they use the json file for dependencies)

Package.json file for dependencies

Package Managers and NOT build tools

Install dependencies, but not used for transpiling JS code


npm install = installs dependencies

NPM Repository for dependencies

Command Line Tool - npm

    npm start - start the application

    npm stop - stop the application

    npm test - run tests locally

    npm publish - publish the artifact


What does the zip/tar file include?

    Application code, but NOT the dependencies

    When running app on a server:

    You must install the dependencies first!

    Unpack zip/tar

    Run the application, using npm run or node command

    Must copy artifact to server and package.json file (can include in a single file - tar)

    npm pack

Package Frontend Code

    Frontend: React

    Backend: Java, Nodejs, Python, etc

Different ways to package this,

    Package frontend and backend code separately

    Common artifact for both


    Separate package.json file for frontend and backend

        Both are JavaScript

    Frontend/React Code needs to be transpiled!

    Browsers don't support latest JS versions or other fancy code decorations, like JSX

    Code needs to be compressed/minified (JSS & CSS)

    Separate tools for the that - Build Tools/Bundler

        Webpack - transpiles / minifies / bundles / compresses the code

            index.html
            index.bundle.js
            index.bundle.js.map
            style.bundle.css
            favicon.ico

    npm run build

        creates server.bundle.js

    
    Dependencies for Frontend Code

        mpm and yarn for dependencies

        decide on Separate package.json vs common package.json with backend code

        Can be an advantage to have same build and package management tools


    Package Frontend Code

        Bundle frontend App with Webpack

        Manage dependencies with npm or yarn


        Package everything into a WAR files








# Usage

  michaelbradley@Michaels-iMac-2 node-app % npm pack
npm notice
npm notice 📦  nodejs-app@1.0.0
npm notice Tarball Contents
npm notice 155B Dockerfile
npm notice 92B Readme.md
npm notice 586B app/server.js
npm notice 313B package.json
npm notice Tarball Details
npm notice name: nodejs-app
npm notice version: 1.0.0
npm notice filename: nodejs-app-1.0.0.tgz
npm notice package size: 796 B
npm notice unpacked size: 1.1 kB
npm notice shasum: 287d36bff5d1d191f25c103b742002ab329b5d76
npm notice integrity: sha512-3cN6K31oOmP7T[...]p5naurQ9I568w==
npm notice total files: 4
npm notice
nodejs-app-1.0.0.tgz
