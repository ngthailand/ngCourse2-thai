# ไดเร็กทีฟ NgClass

ไดเร็กทีฟ `ngClass` ทำหน้าที่เปลี่ยนแอดทริบิวต์ class ของคอมโพเนนท์หรือเอเลเมนท์ที่มี `ngClass` อยู่ `ngClass` สามารถนำไปใช้งานได้หลายวิธี

## การผูกด้วย string

เราสามารถผูก string ตรง ๆ ไปยังแอดทริบิวต์ได้ ซึ่งจะทำงานเหมือนกับแอดทริบิวต์ class ปกติ

```typescript
@Component({
  selector: 'class-as-string',
  template: `
    <p ngClass="centered-text underlined" class="orange">
      <ng-content></ng-content>
    </p>
  `,
  styles: [`
    .centered-text {
      text-align: center;
    }

    .underlined {
      border-bottom: 1px solid #ccc;
    }

    .orange {
      color: orange;
    }
  `]
})
export class ClassAsStringComponent {
}
```

[ดูตัวอย่าง](https://plnkr.co/edit/8M32UVF8BHJDaRMGziFi?p=preview)

อย่างในกรณีนี้เราทำการผูก string ตรง ๆ จึงควรหลีกเลี่ยงการใช้ [] นอกจากนี้แล้ว `ngClass` ยังสามารถใช้งานร่วมกับแอดทริบิวต์ class ได้ด้วย

## การผูกด้วยอาเรย์

```typescript
@Component({
  selector: 'class-as-array',
  template: `
    <p [ngClass]="['warning', 'big']">
      <ng-content></ng-content>
    </p>
  `,
  styles: [`
    .warning {
      color: red;
      font-weight: bold;
    }

    .big {
      font-size: 1.2rem;
    }
  `]
})
export class ClassAsArrayComponent {
}
```

[ดูตัวอย่าง](https://plnkr.co/edit/8M32UVF8BHJDaRMGziFi?p=preview)

การผูกด้วยอาเรย์นอกจากการผูกด้วยนิพจน์แล้วเราจะต้องหุ้มไดเร็กทีฟด้วย [] ด้วยเช่นกัน การผูกแบบนี้จะมีประโยชน์ในกรณีที่ต้องการแปลงค่าจากอาเรย์ที่เปลี่ยนแปลงได้ไปเป็น class

## การผูกด้วยอ็อบเจกต์

สุดท้ายการผูกด้วยอ็อบเจกต์สามารถทำได้เช่นกัน โดย Angular 2 จะทำการแปลงพร็อปเพอร์ตี้ของอ็อบเจกต์ที่มีค่าเป็น true ไปเป็น class


```typescript
@Component({
  selector: 'class-as-object',
  template: `
    <p [ngClass]="{ card: true, dark: false, flat: flat }">
      <ng-content></ng-content>
      <br/>
      <button type="button" (click)="flat=!flat">Toggle Flat</button>
    </p>
  `,
  styles: [`
    .card {
      border: 1px solid #eee;
      padding: 1rem;
      margin: 0.4rem;
      font-family: sans-serif;
      box-shadow: 2px 2px 2px #888888;
    }

    .dark {
      background-color: #444;
      border-color: #000;
      color: #fff;
    }

    .flat {
      box-shadow: none;
    }
  `]
})
export class ClassAsObjectComponent {
  flat: boolean = true;
}
```

[ดูตัวอย่าง](https://plnkr.co/edit/8mGcM3?p=preview)

จากตัวอย่างจะเห็นว่าพร็อปเพอร์ตี้ `card` และ `flat` ถูกแปลงเป็น class เนื่องจากมีค่าเป็น true แต่ พร็อปเพอร์ตี้ `dark` ที่มีค่าเป็น false จะไม่ถูกแปลง
