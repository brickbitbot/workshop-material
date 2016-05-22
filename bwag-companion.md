# Companion to "Building Web Apps with Go"

Use this companion to the Building Web Apps with Go book as you go through each section. It is meant to clarify any knowledge the author assumes you already have and provide resources for where to learn more should you need it.

As you read through each section of the book, look back here for explanation of terminology, supplemental material and instructions where necessary.

## BWAG: "Prerequisites"

### Ensure Go is installed
You should have already installed Go at the end of the day yesterday. Ask a TA to verify your installation.

Open a terminal (`cmd` on Windows, `Terminal` on Mac OS) and type the command `go version` which, if installed, will output the version of Go you have installed like so:

```
go version go1.6 darwin/amd64
```

Your output might be slightly different depending on your Operating System.

### Ensure Git is installed

Git is a version control system that is used to track and optionally push those changes to other computers. We'll need Git installed for both our own programs, getting packages from remote repositories like GitHub and to deploy our application to Heroku later on.

Some Operating Systems come with Git pre-installed. Check if you already have it installed by switching to your terminal window and entering the comment `git version`, which, if installed, will output the version of Git you have installed like so:

```
git version 2.7.1
```

Your output might be slightly different depending on your version. If Git is not installed on your machine, head over to [https://git-scm.com/downloads](https://git-scm.com/downloads) to download the version appropriate for your Operating System.

## BWAG: "Required Packages"

As you may recall from yesterday, often, we rely on functionality exposed through "packages" in Go. A package like "fmt" for example, is part of Go's "Standard Library", the set of packages that you get out of the box when you install Go. Some packages are written by third party developers (like you!) and need to be downloaded separately in order to be used.

The way to obtain those packages before you can use them in your programs is to "go get" them like so in a terminal:

```
go get github.com/russross/blackfriday
```

Go ahead and `go get` all the packages listed in the table under the "Required Packages" section.

## More Go Fundamentals: Pointers, Structs, Methods & Interfaces

Before we can move into the next section of BWAG, we must first understand some additional fundamental concepts in Go, mainly pointers, structs, methods and interfaces. First up, pointers.

### Pointers

Head over to [https://gobyexample.com/pointers](https://gobyexample.com/pointers), read through the examples and do the following exercises:

1. Create a function `nameVal` that takes a `string` value argument and assigns "Todd" to it.
2. Create a function `namePtr` that takes a `pointer` to a `string` argument and assigns "Nick" to it.
3. In your main function, initialize a variable `s` to a string of your choice, then call on your `strVal` function with `s`, print out the result. Call on your `strPtr` function with `s` again, print out the results. What can we observe?

### Structs

Head over to [https://gobyexample.com/structs](https://gobyexample.com/structs), read through the examples and do the following exercises:

1. Define a struct type for Vehicle. We'd like to track the number of wheels, the color and the top speed (e.g 100 for 100 miles per hour).

2. Create a new Vehicle struct using positional arguments. Print it to the screen. What are some of your observations?

3. Create a new Vehicle struct using one of the named fields. Print it to the screen. What are some of your observations?

4. Print out the number of color and number of wheels from the struct you created in #3.

5. Change the speed of the struct you created in #3 and print the entire struct. What are your observations?

6. Initialize a variable `p` and assign it a point to the previously created Vehicle struct. Can you update the color of the vehicle using the variable `p`? Why do you think that is?

### Interfaces

Head over to [https://gobyexample.com/interfaces](https://gobyexample.com/interfaces), read through the examples and do the following exercise:

Assume you have the following interface:

```go
type entity interface {
  name() string
  kind() string
}
```

and the following function:

```go
func identify(e Entity) {
  fmt.Printf("Entity %s is a %s\n", e.name(), e.kind())
}
```

Your task:

1. Define two structs, `person` and `dog` that each implement the `entity` interface. A `person`'s name will be composed of their `firstName` and `lastName` while a `dog`'s name is just a single `name` property. You may set the `kind` to be either "Human" or "Animal".

2. In your `main` function, call on `identify` with a new `person` struct and again with a new `dog` struct. Each time, you should see a print out of the entity's name and their kind.

3. What can you see as the benefits to using an interface?

4. What happens if you leave out an implementation of a method signature of one of your struct types?

## BWAG: "The `net/http` package"

1. Read the introductory paragraphs for this section.
2. Understand the HTTP Request/Response model (with some white-boarding).
3. Use godoc.org to locate the http `Handler` interface, the `ResponseWriter` and `Request` types.
4. Why do you think the ServeHTTP method takes in a pointer to the `Request` object as opposed to a value?
5. Complete the Line File Server exercise.

## BWAG: "Creating a Basic Web App"

Read through the section and set up the HTML form as instructed. Note the `/markdown` endpoint or route. Let's talk about "routing."

"Route" or "Routing" (also known as request routing or URL dispatching) is mapping URLs to code that handles them.

In `func main()`, we're _handling_ the `markdown` _route_ using the `generateMarkdown` http handler function like so:

```go
func main() {
    http.HandleFunc("/markdown", generateMarkdown)
    http.Handle("/", http.FileServer(http.Dir("public")))
    http.ListenAndServe(":8080", nil)
}

func generateMarkdown(rw http.ResponseWriter, r *http.Request) {
    markdown := blackfriday.MarkdownCommon([]byte(r.FormValue("body")))
    rw.Write(markdown)
}
```

The key concept here is that we have a route and a handler for that route. The handler must accept an `http.ResponseWriter` and an `*http.Request` to satisfy the `http.Handler` interface.

## BWAG: "Deployment"

Let's do a live walkthough of each step in this section.
