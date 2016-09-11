# ไดเร็กทีฟ NgFor

ไดเร็กทีฟ `ngFor` เป็นวิธีในการวนซ้ำเทมเพลต โดยการใช้แต่ละรายการจากการวนซ้ำเป็นบริบทของเทมเพลต

```typescript
@Component({
  selector: 'app',
  directives: [ForExampleComponent],
  template: `
    <for-example *ngFor="let episode of episodes" [episode]="episode">
      {{episode.title}}
    </for-example>
  `
})
export class AppComponent {
  episodes: any[] = [
    { title: 'Winter Is Coming', director: 'Tim Van Patten' },
    { title: 'The Kingsroad', director: 'Tim Van Patten' },
    { title: 'Lord Snow', director: 'Brian Kirk' },
    { title: 'Cripples, Bastards, and Broken Things', director: 'Brian Kirk' },
    { title: 'The Wolf and the Lion', director: 'Brian Kirk' },
    { title: 'A Golden Crown', director: 'Daniel Minahan' },
    { title: 'You Win or You Die', director: 'Daniel Minahan' }
    { title: 'The Pointy End', director: 'Daniel Minahan' }
  ];
}
```
[ดูตัวอย่าง](https://plnkr.co/edit/E2Q8Xi6LATpWcXk6bAUQ?p=preview)

ไดเร็กทีฟ `ngFor` มีรูปแบบแตกต่างจากไดเร็กทีฟอื่น ๆ ที่เราเคยเห็นมา หากคุณคุ้นเคยกับ [for..of](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of) คุณจะพบว่าทั้งสองมีความคล้ายคลึงกัน `ngFor` อนุญาตให้คุณกำหนดอ็อบเจกต์ที่จะได้จากการวนซ้ำ และชื่อที่อ้างอิงถึงแต่ละรายการภายในขอบข่ายในตัวอย่างของเรา คุณสามารถเห็นได้ว่า `episode` สามารถใช้สำหรับการสอดแทรกได้เช่นเดียวกับการผูกสัมพันธ์แอดทริบิวต์ ไดเร็กทีฟดังกล่าวได้ถูกทำการแจงเพิ่มเติม ดังนั้นเมื่อสิ่งนี้ถูกส่งต่อไปยังเทมเพลตจะส่งผลแตกต่างกันเล็กน้อย:


```typescript
@Component({
  selector: 'app',
  directives: [ForExampleComponent],
  template: `
    <template ngFor [ngForOf]="episodes" let-episode>
      <for-example [episode]="episode">
        {{episode.title}}
      </for-example>
    </template>
  `
})
```
[ดูตัวอย่าง](https://plnkr.co/edit/E2Q8Xi6LATpWcXk6bAUQ?p=preview)

สังเกตุว่ามีสิ่งแปลกปลอมอยู่หนึ่งอย่างนั่นคือพร็อปเพอร์ตี้ `let-episode` ที่อยู่บนเอเลเมนท์เทมเพลต ไดเร็กทีฟ `ngFor` ได้จัดเตรียมตัวแปรบางตัวมาเป็นบริบทภายในขอบข่าย `let-episode` เป็นการผูกสัมพันธ์กับบริบทและในที่นี้มันได้หยิบแต่ละรายการของการวนซ้ำมา `ngFor` ยังคงเตรียมตัวแปรอื่น ๆ ที่สามารถถูกผูกได้ดังนี้:

- _index_ - ตำแหน่งของรายการปัจจุบันในแต่ละการวนซ้ำ เริ่มต้นที่ `0`
- _first_ - มีค่าเป็น `true` หากเป็นรายการแรกของการวนซ้ำ
- _last_ - มีค่าเป็น `true` หากเป็นการการสุดท้ายของการวนซ้ำ
- _even_ - มีค่าเป็น `true` หาก `index` เป็นเลขคู่
- _odd_ - มีค่าเป็น `true` หาก `index` เป็นเลขคี่


```typescript
@Component({
  selector: 'app',
  directives: [ForExampleComponent],
  template: `
    <for-example
      *ngFor="let episode of episodes; let i = index; let isOdd = odd"
      [episode]="episode"
      [ngClass]="{ odd: isOdd }">
      {{i+1}}. {{episode.title}}
    </for-example>

    <hr/>

    <h2>Desugared</h2>

    <template ngFor [ngForOf]="episodes" let-episode let-i="index" let-isOdd="odd">
      <for-example [episode]="episode" [ngClass]="{ odd: isOdd }">
        {{i+1}}. {{episode.title}}
      </for-example>
    </template>
  `
})
```
[ดูตัวอย่าง](https://plnkr.co/edit/GaxhVSjfY8UmHm4T3PNg?p=preview)

## trackBy ##

บ่อยครั้งที่ `ngFor` ถูกใช้เพื่อวนซ้ำแต่ละรายการจากรายการของอ็อบเจกต์ด้วยฟิลด์ ID ที่เป็นเอกลักษณ์ แต่ในเคสนี้ เราสามารถใช้ฟังก์ชัน `trackBy` ที่ช่วยให้ Angular สามารถติดตามรายการต่าง ๆ ภายในรายการได้ ดังนั้นแล้วมันสามารถตรวจพบรายการที่ถูกเพิ่มหรือถอนได้และช่วยเพิ่มประสิทธิภาพอีกด้วย

Angular 2 จะพยายามที่จะติดตามอ็อบเจกต์จากการอ้างอิงเพื่อกำหนดว่ารายการไหนควรจะถูกสร้างหรือทำลาย อย่างไรก็ตามหากคุณได้แทนที่รายการเดิมด้วยรายการจากแหล่งใหม่ บางครั้งเป็นผลลัพธ์ของการร้องขอจาก API เราสามารถที่จะเพิ่มประสิทธิภาพได้โดยการบอก Angular 2 ว่าเราควรจะอ้างอิงรายแต่ละรายการอย่างไร

ดังตัวอย่าง ถ้าปุ่ม `Add Episode` เป็นการร้องขอรายการของ `episodes` ชุดใหม่ เราอาจะไม่ต้องการทำลายทุก ๆ รายการในรายการแล้วจึงสร้างทั้งหมดขึ้นมาใหม่ ถ้าแต่ละ `episode` มี ID ที่เป็นเอกลักษณ์อยู่แล้ว เราสามารถเพิ่มฟังก์ชัน `trackBy` ได้:

```typescript
@Component({
  selector: 'app',
  directives: [ForExampleComponent],
  template: `
  <button (click)="addOtherEpisode()" [disabled]="otherEpisodes.length === 0">Add Episode</button>
    <for-example
      *ngFor="let episode of episodes;  
      let i = index; let isOdd = odd;  
      trackBy: trackById" [episode]="episode"
      [ngClass]="{ odd: isOdd }">
      {{episode.title}}
    </for-example>


  `
})
export class AppComponent {

  otherEpisodes: any[] = [
    { title: 'Two Swords', director: 'D. B. Weiss', id: 8 },
    { title: 'The Lion and the Rose', director: 'Alex Graves', id: 9 },
    { title: 'Breaker of Chains', director: 'Michelle MacLaren', id: 10 },
    { title: 'Oathkeeper', director: 'Michelle MacLaren', id: 11 }]

  episodes: any[] = [
    { title: 'Winter Is Coming', director: 'Tim Van Patten', id: 0 },
    { title: 'The Kingsroad', director: 'Tim Van Patten', id: 1 },
    { title: 'Lord Snow', director: 'Brian Kirk', id: 2 },
    { title: 'Cripples, Bastards, and Broken Things', director: 'Brian Kirk', id: 3 },
    { title: 'The Wolf and the Lion', director: 'Brian Kirk', id: 4 },
    { title: 'A Golden Crown', director: 'Daniel Minahan', id: 5 },
    { title: 'You Win or You Die', director: 'Daniel Minahan', id: 6 }
    { title: 'The Pointy End', director: 'Daniel Minahan', id: 7 }
  ];

  addOtherEpisode() {
    // We want to create a new object reference for sake of example
    let episodesCopy = JSON.parse(JSON.stringify(this.episodes))
    this.episodes=[...episodesCopy,this.otherEpisodes.pop()];
  }
  trackById(index: number, episode: any): number {
    return episode.id;
  }
}
```

เพื่่อจะรู้ว่ามันส่งผลกระทบต่อวิธีการสร้างคอมโพเนนต์ `ForExample` อย่างไร เรามาลองเพิ่มการล็อกดู

```typescript
export class ForExampleComponent {
  @Input() episode;

  ngOnInit() {
    console.log('component created', this.episode)
  }
  ngOnDestroy() {
    console.log('destroying component', this.episode)
  }
}
```
[ดูตัวอย่าง](https://plnkr.co/edit/bjqil7?p=preview)

เมื่อเราได้เห็นตัวอย่างดังกล่าว เราคลิกบนปุ่ม `Add Episode` เราเห็นได้ว่าผลลัพธ์ในคอนโซลแสดงให้เห็นว่ามีแค่คอมโพเนนต์เดียวเท่านั้นที่ถูกสร้างใหม่

หากเราลองลบ `trackBy` ออกจาก `*ngFor` ทุกครั้งที่เรากดปุ่ม เราจะเห็นทุก ๆ ไอเท็มในรายการถูกทำลายและสร้างใหม่  

[ดูตัวอย่างที่ไม่มี trackBy](https://plnkr.co/edit/CbPDig?p=preview)
