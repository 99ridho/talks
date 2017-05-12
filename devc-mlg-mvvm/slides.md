### Architecting iOS Apps with MVVM + RxSwift

FB Dev C: Malang Meetup May 2017

---

### Hello!

I'm Ridho

iOS Developer @ Gumcode

Part of BCC FILKOM UB Family

---

### It starts from a problem

---

### MVC (expectation)

![aaa](https://cdn-images-1.medium.com/max/600/1*c0aGaDNX41qu6e8E4OEgwQ.png)

---

### Reality :(

![aaa](https://cdn-images-1.medium.com/max/800/1*PkWjDU0jqGJOB972cMsrnA.png)

---

### So, there's a joke

Massive View Controller :")

---

### Massive View Controller

* Everything is done from `UIViewController`
* Bloated
* Hard to test
* Hard to maintain

---

### So, what is MVVM?

---

### MVVM

Model - View - ViewModel

---

### ViewModel

* Processing user input
* Altering model
* Handle data presentation & prepare them
* Provide data bindings to view

---

![aaa](https://cdn-images-1.medium.com/max/800/1*uhPpTHYzTmHGrAZy8hiM7w.png)

---

### Rule of thumb of Viewmodel

* No `import UIKit`
* No references to View
* No references to `UIKit` components such as `UIButton`, `UILabel`, etc.
* Only data

---

### Bindings?

We use RxSwift :D

---

![aaa](https://raw.githubusercontent.com/ReactiveX/reactivex.github.io/develop/assets/Rx_Logo_S.png)

### RxSwift is ...

* Swift version of Rx
* API for asynchronous programming with observable streams

---

### Observable in Rx...

* Can emit the item/s that can be subscribed by observer
* Sequence of items that Observable emits

![aaa](http://reactivex.io/assets/operators/legend.png)

---

```swift
struct RepositoryListViewModel {
    
    let githubAPIClient = GithubAPIClient()
    
    func getReposName(username: Observable<String>) -> Observable<[(String, String)]> {
        return githubAPIClient
            .findRepos(withUsername: username)
            .map { repos in
                return repos.map { repo in
                    return (
                        name: "\(repo.ownerUsername)/\(repo.name)",
                        detail: "\(repo.starCount) stars - \(repo.forkCount) forks"
                    )
                }
            }
    }
    
}
```

---

```swift
class RepositoryListViewController: UIViewController {
    // 
    // attributes, viewdidload, rx and view setup, truncated for space
    //
    func setupRx() {
        repositoryListViewModel.getReposName(username: self.username)
            .observeOn(MainScheduler.instance)
            .catchError({ error in
                self.showAlert(withMessage: "Error occured, maybe internet problem??")
                return Observable.just([])
            })
            .bind(to: repoTableView.rx.items) { (tableView, row, element) in
                let cell = UITableViewCell(
                    style: UITableViewCellStyle.subtitle, 
                    reuseIdentifier: "repoCell"
                )
                
                let (repoName, detail) = element
                cell.textLabel?.text = repoName
                cell.detailTextLabel?.text = detail
                
                return cell
            }
            .addDisposableTo(disposeBag)
    }
}
```

---

![aa](https://media.giphy.com/media/3ohzdFZTPZlj4F9JpS/source.gif)

---

### Pros

* Bindings (yeay)
* View become less bloated
* Easy to test the business logic & data layer
* View & ViewModel loosely coupled

---

### Cons

* Beware of **Massive ViewModel** :")
* View isn't completely passive, still used for navigation

---

### Thanks!

*"commitment to write a clean code should be started from an early stage of SDLC"*

*(Ridho, 2017)*