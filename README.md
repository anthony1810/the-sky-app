# TheSkyApp
This project is created as a code Challenge for Async/ Await Course - [Unit 1.6 - Task and TaskGroup.](https://coda.io/d/iOSMentorship-io_dkx_nXWeciu/Unit-1-6-Task-and-Task-Group_suUKj#_luhdI)


## Getting started
1. Clone this project
2. Checkout a new branch with your name
3. Finish the code challenge 
4. Submit an MR and assign that to me

## About
This SkyApp is a scanner to scan for alien life outside the earth orbit. Everytime you tap engage the system will run a `ScanTask` to find alien life. However the system is running sequentially so only one `ScanTask` can run at all time. Your job is to make this system utilize its hardware capability and run as many as possible with `TaskGroup`.

### Challenge ###
1. Open `ScanTask` and look at the implementation of how a single task will be run in `run` method.
2. Open `ScanModel` and look at `Worker` function which its core is a `ScanTask` and a bunch of other UI update things
3. Now look at `runAllTasks` function and your challenges will be revolved around this function

**Your Fist Challenge**
- Create a `TaskGroup` inside `runAllTasks` so that all `ScanTask` will run concurrently. Be advised that max `ScanTask` this system can run concurrently is four. 
- Remember to set priority, otherwise it will be called on main thread.
- Finally, when all tasks finished, run this to update the UI:
```
await MainActor.run {
  completed = 0
  countPerSecond = 0
  scheduled = 0
}
```

**Your Second Challenge**
- Open `ScanTask` and change function `run` to this so it can throw error:
```
func run() async throws -> String {
```
- Add this line to the top of the above function:
```
try await UnreliableAPI.shared.action(failingEvery: 10)
```
- Now this function will throw error every 10 seconds, your job is to adjust your codebase to cope with this and propagate error correctly upward. 
- Error handling with async/ await is the same with Swift native one, so it shouldn't be any problem for you. Just use the proper TaskGroup creation that can throw error. 

**Your Third Challenge***
- Notice that your tasks will be abruptly stopped when an error is thrown. Your job is to change how ScanTask return result so TaskGroup can continue regardless of error throw by individual task. 
```
let result: Result<String, Error>
do {
    result = try .success(await task.run())
} catch {
    result = .failure(error)
}
  ```

Good Luck! and don't forget to submit an MR.
