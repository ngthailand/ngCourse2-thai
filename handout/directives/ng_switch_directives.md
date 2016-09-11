# ไดเร็กทีฟ NgSwitch

จริง ๆ แล้ว `ngSwitch` ประกอบด้วยไดเร็กทีฟ 2 ประเภท คือไดเร็กทีฟแบบแอดทริบิวต์และ ไดเร็กทีฟแบบโครงสร้าง `ngSwitch` มีความคล้ายคลึงกับ [switch statement](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Statements/switch) ในภาษาจาวาสคริปต์และภาษาโปรแกรมอื่น ๆ แต่เป็นในรูปแบบเทมเพลต

```typescript
@Component({
  selector: 'app',
  directives: [DoorComponent],
  template: `
    <div [ngSwitch]="door">
      <door [id]="1" *ngSwitchCase="1">A new car!</door>
      <door [id]="2" *ngSwitchCase="2">A washer and dryer!</door>
      <door [id]="3" *ngSwitchCase="3">A trip to Tahiti!</door>
      <door [id]="4" *ngSwitchCase="4">25 000 dollars!</door>
      <door *ngSwitchDefault class="closed"></door>
    </div>
    
    <div class="options">
      <input type="radio" name="door" (click)="setDoor(1)" /> Door 1
      <input type="radio" name="door" (click)="setDoor(2)" /> Door 2
      <input type="radio" name="door" (click)="setDoor(3)" /> Door 3
      <input type="radio" selected="selected" name="door" (click)="setDoor()"/> Close all
    </div>
  `
})
export class AppComponent {
  door: number;
  
  setDoor(num: number) {
    this.door = num;
  }
}
```
[ดูตัวอย่าง](https://plnkr.co/edit/n3jhb6BnRuU17uxIYfMT?p=preview)

ในที่นี้เราจะเห็นไดเร็กทีฟ `ngSwitch` ติดกับเอเลเมนท์ นิพจน์ดังกล่าวจะผูกกับข้อกำหนดของไดเร็กทีฟที่จะถูกนำไปเปรียบเทียบต่อในไดเร็กทีฟแบบโครงสร้างกรณีที่นิพจน์ใน `ngSwitchCase` ของคอมโพเนนท์ใดเข้าคู่กับที่ส่งไปใน `ngSwitch` คอมโพเนนท์นั้นจะถูกสร้างและคอมโพเนนท์อื่นจะถูกทำลาย ถ้าหากไม่มีเคสใด ๆ เข้าคู่คอมโพเนนท์ที่ผูกกับ `ngSwitchDefault` จะถูกสร้างขึ้นและคอมโพเนนท์อื่นจะถูกทำลาย

ควรทราบไว้ว่าคอมโพเนนท์หลาย ๆ อันสามารถที่จะเข้าคู่พร้อมกันได้ผ่านการใช้ `ngSwitchCase` และคอมโพเนนท์ดังกล่าวจะถูกสร้างขึ้นทั้งหมด เนื่องจากคอมโพเนนท์จะถูกสร้างและทำลายควรระมัดระวังถึงความสิ้นเปลืองในการใช้งาน