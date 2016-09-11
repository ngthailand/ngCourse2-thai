# ไดเร็กทีฟแบบโครงสร้าง (Structural Directives)

ไดเร็กทีฟแบบโครงสร้างเป็นวิธีในการควบคุมการแสดงผลของคอมโพเนนท์หรือเอเลเมนท์ผ่านการใช้แท็ก `template`  ทำให้เราสามารถรันโค๊ดที่จะเป็นตัวกำหนดการแสดงผลในท้ายที่สุดได้ Angular 2 ได้ติดตั้งไดเร็กทีฟแบบโครงสร้างบางตัวมาด้วย เช่น `ngIf`, `ngFor` และ `ngSwitch`

*Note: สำหรับผู้ที่ไม่คุ้นเคยกับแท็ก `template`, มันเป็นเอเลเมนท์ของ HTML ที่มีพร็อปเพอร์ตี้พิเศษบางตัวอยู่ คอนเทนต์ภายในแท็ก `template`จะไม่ถูกแสดงผลทันทีหลังจากโหลดหน้าเว็บ แต่จะถูกโหลดด้วยโค๊ดผ่าน Runtime เท่านั้น สามารถหาข้อมูลเพิ่มเติมเกี่ยวกับแท๊ก `template` ได้ที่ [MDN documentation](https://developer.mozilla.org/en/docs/Web/HTML/Element/template)*.

ไดเร็กทีฟแบบโครงสร้าง มีซินแท็กพิเศษของตนเองทำงานเป็น syntactic sugar ดังเทมเพลตด้านล่าง

```typescript
@Component({
    selector: 'directive-example',
    template: `
        <p *structuralDirective="expression">
            Under a structural directive.
        </p>
    `
})
```

นอกจากการหุ้มด้วย [] แล้วไดเร็กทีฟแบบโครงสร้างยังสามารถนำหน้าด้วยดอกจันได้ดังตัวอย่าง แต่ยังคงใช้นิพจน์ในการผูกเหมือนเดิมแม้ว่าจะไม่ได้ใช้ [] เป็นความจริงที่ว่า syntactic suger ของมันทำให้สามารถใช้งานไดเร็กทีฟได้ง่ายขึ้น และใกล้เคียงกับการใช้งานใน Angular 1 คอมโพเนนท์ในตัวอย่างด้านบนทำงานแบบเดียวกันกับตัวอย่างต่อไปนี้:

```typescript
@Component({
    selector: 'directive-example',
    template: `
        <template [structuralDirective]="expression">
            <p>
                Under a structural directive.
            </p>
        </template>
    `
})
```


ในทีนี้เราจะได้เห็นสิ่งที่กล่าวไว้ก่อนหน้านี้ เมื่อเรากล่าวไว้ว่าไดเร็กทีฟแบบโครงสร้างใช้งานแท็กเทมเพลต Angular 2 มีไดเร็กทีฟ `template` ที่ทำงานแบบเดียวกันอยู่:

```typescript
@Component({
    selector: 'directive-example',
    template: `
        <p template="structuralDirective expression">
            Under a structural directive.
        </p>
    `
})
```
