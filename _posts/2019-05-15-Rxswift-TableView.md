---
layout: post
title: RxSwift로 tableview사용해보기
sitemap :
  changefreq : daily
  priority : 1.0
---

- RxSwift로 tableview구성하기

```c
import UIKit
import RxSwift
import RxCocoa
import RxDataSources // 기본 테이블뷰는 상관없지만 section을 사용하기 위해서 추가

struct CustomData {
    var num: Int
    var content: String
}

struct SectionOfCustomData {
    var header: String
    var footer: String
    var items: [Item]
}
extension SectionOfCustomData: SectionModelType {
    typealias Item = CustomData
    
    init(original: SectionOfCustomData, items: [Item]) {
        self = original
        self.items = items
    }
}

class TableViewController: UIViewController, UITableViewDelegate {

    @IBOutlet weak var tableView: UITableView!
    
    let disposeBag = DisposeBag()

    override func viewDidLoad() {
        super.viewDidLoad()
        
        tableView.delegate = self
        
        let dataSource = RxTableViewSectionedReloadDataSource<SectionOfCustomData>(
            configureCell: { dataSource, tableView, indexPath, item in
                let cell = tableView.dequeueReusableCell(withIdentifier: "Cell", for: indexPath)
                cell.textLabel?.text = "Item \(item.num): \(item.content)"
                return cell
        })

        dataSource.titleForHeaderInSection = { dataSource, index in
            return dataSource.sectionModels[index].header
        }

        dataSource.titleForFooterInSection = { dataSource, index in
            return dataSource.sectionModels[index].footer
        }

        dataSource.canEditRowAtIndexPath = { dataSource, indexPath in
            return true
        }

        dataSource.canMoveRowAtIndexPath = { dataSource, indexPath in
            return true
        }

        let sections = [
            SectionOfCustomData(header: "section1", footer: "footer1", items: [CustomData(num: 0, content: "111"), CustomData(num: 1, content: "222") ]),
            SectionOfCustomData(header: "section2", footer: "footer2", items: [CustomData(num: 2, content: "111"), CustomData(num: 3, content: "222") ]),
            SectionOfCustomData(header: "section2", footer: "footer3", items: [CustomData(num: 4, content: "111"), CustomData(num: 5, content: "222") ])
        ]

        Observable.just(sections)
            .bind(to: tableView.rx.items(dataSource: dataSource))
            .disposed(by: disposeBag)
        
        tableView.rx
            .modelSelected(CustomData.self)
            .subscribe(onNext: { (data) in
                print(data.content)
            }, onError: { (error) in
                print(error)
            }).disposed(by: disposeBag)
        
    }
    
    func tableView(_ tableView: UITableView, editActionsForRowAt indexPath: IndexPath) -> [UITableViewRowAction]? {
        let delete = UITableViewRowAction(style: .destructive, title: "Delete") { (action, indexPath) in
            // delete item at indexPath
        }
        
        let share = UITableViewRowAction(style: .normal, title: "Disable") { (action, indexPath) in
            // share item at indexPath
        }
        
        share.backgroundColor = UIColor.blue
        
        return [delete, share]
    }
    
}

```

