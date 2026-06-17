# RepoExplorer

## Overview

RepoExplorer is a small SwiftUI application that allows users to search GitHub repositories and view basic repository information.

The goal of this assignment was not to build a production-ready GitHub client but to demonstrate:

* SwiftUI
* Swift 6 Concurrency
* Separation of concerns
* Basic persistence
* Request cancellation
* Product decision making under incomplete requirements

---

## Architecture

The project follows a simple MVVM structure.

### Models

* Repo
* SearchResponse

Responsible for decoding GitHub API responses.

### Networking

* APIClient

Handles communication with GitHub Search API using URLSession and async/await.

### Services

* SearchHistoryService

Responsible for storing and retrieving recent searches from UserDefaults.

### ViewModel

* SearchViewModel

Contains presentation logic, manages search requests, loading state, error state and search history.

### Views

* SearchView
* RepoRowView
* RepoDetailView

Responsible only for rendering UI.

---

## Concurrency

The assignment specifically requested Swift 6 concurrency and request cancellation.

Implementation details:

* URLSession async/await is used for networking.
* ViewModel is isolated using MainActor.
* Existing search requests are cancelled before starting a new request.
* A small debounce delay is applied before firing requests.
* No Combine.
* No DispatchQueue/GCD.

Example scenario:

If the user types:

swift
swiftu
swiftui

Older requests are cancelled and only the latest result is allowed to update the UI.

---

## Persistence

Search history is stored using UserDefaults.

Reasoning:

The requirement only mentions remembering previous searches across launches.

For this amount of data, CoreData would add unnecessary complexity without providing additional value.

The latest 10 searches are retained.

Duplicate entries are removed before saving.

---

## Assumptions

The brief intentionally leaves some product decisions open, so the following assumptions were made:

* Search is performed using GitHub repository search API.
* Search starts automatically when the query changes.
* Search history is device-local.
* Repository details are displayed using data already available in the search response.
* Pagination is not required for the scope of this exercise.
* Authentication is not required.

---

## Questions

Questions I would clarify with a PM or designer before implementation:

1. Should search happen automatically or only after pressing Search?
2. Should recent searches be removable?
3. Should repository bookmarking be supported?
4. Is offline support required?
5. Should results support sorting or filtering?
6. Should repository details include README content, contributors or open issues?

---

## What I Would Improve With More Time

* Unit tests for ViewModel and networking layer
* Pagination support
* Repository caching
* Better loading and error states
* Sorting and filtering
* Accessibility improvements
* Snapshot/UI tests

---

## Testing

Due to the limited scope of the exercise, automated tests were not included.

The first tests I would add:

* API response decoding
* Search cancellation behaviour
* Search history persistence
* ViewModel state transitions
* Error handling scenarios

---

## Running The Project

1. Open the project in Xcode.
2. Select an iOS Simulator.
3. Build and run.

No additional setup is required.
