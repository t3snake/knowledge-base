# Codecrafters local test guide

`Note: This guide only works on linux. successfully was able to do it through wsl (ubuntu) on windows. MacOS has extra steps in repo but I have not tried it`

- Clone [course-sdk](https://github.com/codecrafters-io/course-sdk) from codecrafters.
- Ensure `docker` and `bun` are installed (as per repo instruction)
- Note: `docker` by default only seems to run with sudo, use stack overflow instructions to make it work without sudo
- Run `bun install && make install` to build course-sdk. It is also added to the PATH
- Clone the challenge specific repo eg: [build-your-own-git](https://github.com/codecrafters-io/build-your-own-git)
- `cd` into the repo and run `course-sdk test <language>`
- in case cache issue is there: `course-sdk test --no-compile <language>`
- At the moment it is not connected to your solution.
- `cd solution/<language>`
- inside this folder there will be the first challenge `01-gg7` for eg. this is an index + the 3 character code for the challenge as seen in the codecrafters catalog
- To test a specific challenge, count to the number of the stage you want to test and append the 3 char code. eg: `07-cr7` and within this folder add a `code` dir and add your repo contents
- in short within `solution/go` > `mkdir 07-cr7` > `git clone <your_repo> code`
- And you are ready to test

Note: The course-sdk automatically downloads the tester repo for the challenge such as [sqlite-tester](https://github.com/codecrafters-io/sqlite-tester) which contains the actual tests.