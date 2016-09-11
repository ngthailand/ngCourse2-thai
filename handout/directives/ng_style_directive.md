# ไดเร็กทีฟ NgStyle

Angular 2 ได้เตรียมไดเร็กทีฟมาให้จำนวนหนึ่ง `ngStyle` เป็นไดเร็กทีฟที่ใช้เพื่อแก้ไขแอดทริบิวต์สไตล์ของคอมโพเนนท์หรือเอเลเมนท์ดังตัวอย่าง:


```typescript
@Component({
  selector: 'style-example',
  template: `
    <p  style="padding: 1rem"
        [ngStyle]="{ 
            color: 'red',
            'font-weight': 'bold',
            borderBottom: borderStyle
        }">
        <ng-content></ng-content>
    </p>
  `
})
export class StyleExampleComponent {
  borderStyle: string = '1px solid black';
}
```
[ดูตัวอย่าง](https://plnkr.co/edit/rTNmetnnehENXL3Ungch?p=preview)

โปรดจำไว้ว่าการผูกของไดเร็กทีฟทำงานแบบเดียวกันกับการผูกแอดทริบิวต์ ของคอมโพเนนท์ดั้งนั้นการผูกนิพจน์ หรืออ็อบเจกต์ ไปยัง `ngStyle` จะต้องหุ้ม `ngStyle` ด้วยวงเล็บ ([]) หลังจากนั้น `ngStyle` จะรับพร็อปเพอร์ตี้และค่าของพร็อปเพอร์ตี้ของอ็อบเจกต์มาแปลงเป็นสไตล์ของเอเลเมนท์จากตัวอย่างแสดงให้เห็นว่าการตั้งชื่อพร็อปเพอร์ตี้สามารถใช้ได้ทั้งในรูปแบบ kebab เช่น `font-weight` และ camel เช่น `borderBottom` โปรดจำไว้เช่นกันว่าแอดทริบิวต์สไตล์และ `ngStyle` ของ Angular 2 จะถูกใช้งานร่วมกันในการตกแต่งเอเลเมนท์