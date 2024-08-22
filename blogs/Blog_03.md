
## Một cách suy nghĩ khác về TypeScript

### Kiểu Dữ Liệu -> Tập Hợp

Hệ thống kiểu dữ liệu của TypeScript có thể được coi là một ngôn ngữ thuần túy chức năng hoạt động trên các kiểu dữ liệu. Nhưng điều đó có nghĩa là gì khi nói về việc thao tác trên một kiểu dữ liệu? Đối với mình, việc giải quyết các kiểu dữ liệu thành tập hợp các giá trị mà nó có thể tạo ra rất hữu ích. Tập hợp này sẽ chứa mọi giá trị thực sự có thể gán cho kiểu dữ liệu đó.

Khi đó, cú pháp cốt lõi của TypeScript là chức năng để thao tác trên các phần tử trong bất kỳ tập hợp nào, giống như cách bạn có thể thao tác trên một tập hợp thực sự trong một ngôn ngữ lập trình thông thường.

Bởi vì TypeScript là một hệ thống kiểu dữ liệu dựa trên cấu trúc, không phải dựa trên danh nghĩa, nên "tập hợp" mà kiểu dữ liệu tạo ra có thể hữu ích hơn so với định nghĩa kiểu dữ liệu thực tế (nhưng không phải lúc nào cũng vậy).

Nếu chúng ta nghĩ về mỗi kiểu dữ liệu như là tập hợp của các giá trị thực tế mà nó có thể tạo ra, thì chúng ta có thể nói rằng một chuỗi ký tự (string) chỉ là tập hợp vô hạn của mọi hoán vị của các ký tự, hoặc một số (number) là tập hợp vô hạn của mọi hoán vị của các chữ số.

Khi bạn bắt đầu suy nghĩ về hệ thống kiểu dữ liệu như một ngôn ngữ lập trình chức năng thực thụ dành riêng cho việc xử lý các tập hợp, các tính năng nâng cao sẽ trở nên dễ hiểu hơn.

Bài viết này sẽ trình bày hầu hết các tính năng của TypeScript qua lăng kính: các kiểu dữ liệu là các tập hợp mà chúng có thể tạo ra và TypeScript là một ngôn ngữ lập trình chức năng hoạt động trên các tập hợp.

Lưu ý rằng mình không ngụ ý rằng các tập hợp và kiểu dữ liệu là tương đương nhau, chúng không phải vậy.

### Phân tích các kiểu dữ liệu cơ bản của TypeScript

#### Giao (Intersection) (&)

Giao (Intersection) là một ví dụ tuyệt vời mà mô hình tư duy này giúp bạn suy nghĩ tốt hơn về thao tác này. Hãy xem ví dụ sau:

```typescript
type Bar = { x: number };
type Baz = { y: number };
type Foo = Bar & Baz;
```

Chúng ta đang giao giữa `Bar` và `Baz`. Ý nghĩ đầu tiên của bạn có thể là thao tác giao đã được áp dụng theo cách sau:

```
Giao giữa {x: number} và {y: number}
```

Chúng ta xác định sự chồng chéo giữa hai đối tượng và lấy đó làm kết quả. Nhưng... không có sự chồng chéo nào? Bên trái chỉ có `x`, và bên phải chỉ có `y`, mặc dù cả hai đều là số. Vậy tại sao kết quả giao lại tạo ra một kiểu dữ liệu cho phép điều này:

```typescript
let x: Foo = { x: 2, y: 2 };
```

Một cách dễ hiểu hơn để suy nghĩ về những gì đang xảy ra là giải quyết các kiểu dữ liệu `Bar` và `Baz` thành các tập hợp mà chúng tạo ra, không phải những gì mà văn bản biểu thị.

Khi chúng ta định nghĩa một kiểu dữ liệu là `{ y: number }`, chúng ta có thể tạo ra một tập hợp vô hạn của các đối tượng mà ít nhất có thuộc tính `y` trong đó, với `y` là một số:

```
Tập hợp của { y: number }
```

Lưu ý: Chú ý cách mình nói "tập hợp của các kiểu đối tượng mà ít nhất có thuộc tính `y` trong đó". Đó là lý do tại sao các thuộc tính khác ngoài `y` tồn tại trong một số kiểu đối tượng. Nếu bạn có một biến kiểu `{y: number}`, điều đó không quan trọng nếu đối tượng đó có nhiều thuộc tính hơn ngoài `y`, do đó TypeScript cho phép điều này.

Bây giờ khi chúng ta biết cách thay thế các kiểu dữ liệu bằng các tập hợp mà chúng tạo ra, thao tác giao sẽ trở nên dễ hiểu hơn:

```
Giao giữa hai tập hợp
```

#### Hợp (Union)

Sử dụng mô hình tư duy trước đó mà chúng ta đã thiết lập, điều này trở nên đơn giản, chúng ta chỉ cần lấy hợp của hai tập hợp để có tập hợp mới.

```typescript
type Foo = { x: number };
type Baz = { y: number };
type Bar = Foo | Baz;
```

### Khám Phá Kiểu Dữ Liệu (Type Introspection)

Bởi vì các nhà phát triển của TypeScript nghĩ rằng sẽ tiện lợi nếu tích hợp sẵn các nguyên mẫu để chúng ta có thể khám phá các tập hợp này. Ví dụ, chúng ta có thể kiểm tra xem một tập hợp có phải là tập hợp con của một tập hợp khác không và trả về một tập hợp mới trong trường hợp đúng/sai bằng cách sử dụng từ khóa `extends`.

```typescript
type IntrospectFoo = number | null | string extends number
  ? "number | null | string tạo ra một tập hợp là tập hợp con của number"
  : "number | null | string tạo ra một tập hợp không phải là tập hợp con của number";
```

```typescript
// IntrospectFoo = "number | null | string không phải là tập hợp con của number"
```

Ở đây, chúng ta đang kiểm tra xem tập hợp bên trái của từ khóa `extends` có phải là tập hợp con của tập hợp bên phải hay không.

Điều này khá mạnh mẽ vì chúng ta có thể lồng ghép chúng tùy ý.

```typescript
type Foo = null
type IntrospectFoo = Foo extends number | null
  ? Foo extends null
    ? "Foo tạo ra một tập hợp là tập hợp con của null"
    : "Foo tạo ra một tập hợp của number"
  : "Foo tạo ra một tập hợp không phải là tập hợp con của number | null";
```

```typescript
// Kết quả = "Foo tạo ra một tập hợp là tập hợp con của null"
```

Nhưng mọi thứ trở nên kỳ lạ khi chúng ta sử dụng các tham số kiểu và truyền các hợp (union) làm đối số kiểu. TypeScript quyết định thực hiện kiểm tra tập hợp con cho từng phần tử trong hợp riêng lẻ khi sử dụng các tham số kiểu, thay vì giải quyết hợp thành một tập hợp đã tạo trước.

Vì vậy, khi thay đổi ví dụ trước đó để sử dụng các tham số kiểu:

```typescript
type IntrospectT<T> = T extends number | null
  ? T extends null
    ? "T tạo ra một tập hợp là tập hợp con của null"
    : "T tạo ra một tập hợp của number"
  : "T tạo ra một tập hợp không phải là tập hợp con của number | null";
type Result = IntrospectT<number | string>;
```

TypeScript sẽ biến `Result` thành:

```typescript
type Result = IntrospectFoo<number> | IntrospectFoo<string>;
```

Kết quả sẽ được giải quyết thành:

```
"T tạo ra một tập hợp chỉ chứa số" | "T tạo ra một tập hợp với các phần tử không thuộc về number | null";
```

Điều này đơn giản là vì điều này thuận tiện hơn cho hầu hết các thao tác. Tuy nhiên, chúng ta có thể buộc TypeScript không làm điều này bằng cách sử dụng cú pháp tuple.

```typescript
type IntrospectFoo<T> = [T] extends [number | null]
  ? T extends null
    ? "T tạo ra một tập hợp là tập hợp con của null"
    : "T tạo ra một tập hợp của number"
  : "T tạo ra một tập hợp không phải là tập hợp con của number | null";
type Result = IntrospectFoo<number | string>;
// Kết quả = "T tạo ra một tập hợp không phải là tập hợp con của number | null"
```

Điều này là vì chúng ta không còn áp dụng kiểu điều kiện trên một hợp, chúng ta đang áp dụng nó cho một tuple có chứa một hợp bên trong.

Trường hợp biên này rất quan trọng vì nó cho thấy mô hình tư duy của việc luôn giải quyết các kiểu dữ liệu thành các tập hợp mà chúng tạo ra ngay lập tức không phải lúc nào cũng hoàn hảo.


### Ánh Xạ Kiểu Dữ Liệu (Type Mapping)

Trong một ngôn ngữ lập trình thông thường, bạn có thể lặp qua một tập hợp (dù là cách nào trong ngôn ngữ đó) để tạo ra một tập hợp mới. Ví dụ, trong Python nếu bạn muốn làm phẳng một tập hợp các tuple, bạn có thể làm như sau:

```python
nested_set = {(1,3,5,6),(1,2,3,8), (9,10,2,1)}
flattened_set = {}
for tup in nested_set:
  for integer in tup:
    flattened_set.add(integer)
```

Mục tiêu của chúng ta là làm điều này trong các kiểu dữ liệu của TypeScript. Nếu chúng ta nghĩ về:

```typescript
Array<number>
```

như là tập hợp của tất cả các hoán vị của các mảng chứa số:

```
Tập hợp của các mảng chứa số
```

Chúng ta muốn áp dụng một số biến đổi để chọn các số từ mỗi phần tử và đặt chúng vào tập hợp:

```typescript
type InsideArray<T> = T extends Array<infer R>
  ? R
  : "T không phải là tập hợp con của Array<unknown>";
type TheNumberInside = InsideArray<Array<number>>;
// TheNumberInside = number
```

Câu lệnh này thực hiện những điều sau:

1. Kiểm tra xem `T` có phải là tập hợp con của tập hợp mà `Array<any>` tạo ra (lúc này `R` chưa tồn tại, vì vậy chúng ta thay thế nó bằng `any`).
2. Nếu đúng, với mỗi mảng trong tập hợp mà `T` tạo ra, đặt các phần tử của mỗi mảng vào một tập hợp mới gọi là `R'`.
3. Suy luận kiểu nào sẽ tạo ra `R'`, và đặt kiểu đó vào trong `R`, nơi `R` chỉ khả dụng trong nhánh đúng.
4. Trả về `R` như là kiểu cuối cùng.
5. Nếu không, cung cấp thông báo lỗi.

Lưu ý rằng điều này không dựa trên một đặc tả về cách `infer` được triển khai, đây chỉ là một cách để suy nghĩ về cách `infer` hoạt động với mô hình tư duy về tập hợp.

Chúng ta có thể mô tả quá trình này một cách trực quan như sau:

```
Ánh xạ qua các tập hợp để lấy ra kiểu của phần tử trong mảng
```

Với mô hình tư duy này, việc TypeScript sử dụng từ "infer" thực sự có ý nghĩa. Nó tự động tìm ra một kiểu sẽ mô tả cách tạo ra tập hợp mà chúng ta đã tạo ra - `R`.

### Biến Đổi Kiểu Dữ Liệu (Type Transformation) - Các Kiểu Dữ Liệu Được Ánh Xạ (Mapped Types)

Chúng ta vừa mô tả cách mà TypeScript cho phép chúng ta kiểm tra rất chính xác liệu một tập hợp có giống như một cái gì đó không, và ánh xạ chúng dựa trên điều đó. Tuy nhiên, sẽ rất hữu ích nếu chúng ta có thể diễn tả rõ hơn về việc từng phần tử trong một tập hợp được tạo ra bởi một kiểu dữ liệu sẽ trông như thế nào. Nếu chúng ta có thể mô tả tốt tập hợp này, chúng ta có thể tạo ra bất kỳ thứ gì:

- Parser SQL trong các kiểu dữ liệu của TypeScript
- Parser GraphQL trong các kiểu dữ liệu của TypeScript

Các kiểu được ánh xạ (mapped types) là một ví dụ tốt về điều này, và có một ứng dụng ban đầu rất đơn giản: ánh xạ qua từng phần tử trong tập hợp để tạo ra một kiểu đối tượng.

Ví dụ:

```typescript
type OnlyBoolsAndNumbers = {
  [key: string]: boolean hoặc number;
};
```

Bước cuối cùng sẽ được thực hiện trong tâm trí của chúng ta - ánh xạ kiểu đối tượng trở lại thành một tập hợp.

Chúng ta cũng có thể ánh xạ qua một tập hợp con của các chuỗi ký tự:

```typescript
type SetToMapOver = "string" | "bar";
type Foo = { [K in SetToMapOver]: K };
```

Ở đây chúng ta ánh xạ qua tập hợp `["string", "bar"]` để tạo ra một kiểu đối tượng => `{string: "string", bar: "bar"}` mà sau đó mô tả một tập hợp có thể được tạo ra.

Chúng ta có thể thực hiện tính toán cấp kiểu tùy ý trên khóa và giá trị của kiểu đối tượng:

```typescript
type SetToMapOver = "string" | "bar";
type FirstCharacter<T> = T extends `${infer R}${infer _}` ? R : never;
type Foo = {
  [K in SetToMapOver as `IM A ${FirstCharacter<K>}`]: FirstCharacter<K>;
};
```

Lưu ý: `never` là tập hợp rỗng - không có giá trị nào tồn tại trong tập hợp - vì vậy một giá trị với kiểu `never` không thể gán bất cứ thứ gì.

Bây giờ chúng ta đã ánh xạ qua tập hợp `["string", "bar"]` để tạo ra kiểu mới => `{["IM A s"]: "s", ["IM A b"]: "b"}`.

### Logic Lặp Đi Lặp Lại

Điều gì sẽ xảy ra nếu chúng ta muốn thực hiện một số biến đổi đối với một tập hợp, nhưng biến đổi đó khá khó để biểu diễn. Nó cần phải chạy tính toán bên trong của mình một số lần tùy ý trước khi chuyển sang phần tử tiếp theo. Trong một ngôn ngữ lập trình thời gian chạy, chúng ta sẽ dễ dàng sử dụng vòng lặp. Nhưng vì hệ thống kiểu của TypeScript là một ngôn ngữ chức năng, chúng ta sẽ sử dụng đệ quy.

```typescript
type FirstLetterUppercase<T extends string> =
  T extends `${infer R}${infer RestWord} ${infer RestSentence}`
    ? `${Uppercase<R>}${RestWord} ${FirstLetterUppercase<RestSentence>}` // Gọi đệ quy
    : T extends `${infer R}${infer RestWord}`
    ? `${Uppercase<R>}${RestWord}` // Trường hợp cơ bản
    : never;
type UppercaseResult = FirstLetterUppercase<"upper case me">
// UppercaseResult = "Upper Case Me"
```

Bây giờ đầu tiên... haha cái gì vậy. Điều này có thể trông khá phức tạp, nhưng nó chỉ là mã lệnh dày đặc, không phải là phức tạp. Hãy viết một phiên bản TypeScript thời gian chạy để mở rộng những gì đang diễn ra:

```typescript
const separateFirstWord = (t: string) => {
  const [firstWord, ...restWords] = t.split(" ");
  return [firstWord, restWords.join(" ")];
};
const firstLetterUppercase = (t: string): string => {
  if (t.length === 0) {
    // Trường hợp cơ bản
    return "";
  }
  const [firstWord, restWords] = separateFirstWord(t);
  return `${firstWord[0].toUpperCase()}${firstWord.slice(1)} ${firstLetterUppercase(restWords)}; // Gọi đệ quy
};
```

Chúng ta lấy từ đầu tiên của câu hiện tại, viết hoa chữ cái đầu tiên của từ, và sau đó làm tương tự với các từ còn lại, nối chúng lại trong quá trình này.

So sánh ví dụ thời gian chạy với ví dụ cấp kiểu:

- Các câu lệnh `if` để tạo ra trường hợp cơ bản được thay thế bằng các kiểm tra tập hợp con (extends).
- Điều này trông rất giống một câu lệnh `if` bởi vì mỗi tập hợp được tạo ra bằng cách sử dụng `infer` (R, RestWord, RestSentence) chỉ chứa một chuỗi ký tự duy nhất.
- Việc tách một câu thành từ đầu tiên và phần còn lại của câu, sử dụng destructuring, được thay thế bằng ánh xạ `infer` qua 3 tập hợp - `${infer R}${infer RestWord} ${infer RestSentence}`.
- Các tham số hàm được thay thế bằng các tham số kiểu.
- Các lệnh gọi hàm đệ quy được thay thế bằng các khởi tạo kiểu đệ quy.

Chúng ta có khả năng mô tả bất kỳ tính toán nào bằng các khả năng này (hệ thống kiểu là turing hoàn chỉnh).

### Kết Luận

Nếu bạn có thể nghĩ về TypeScript như là một cách rất biểu cảm để thao tác trên các tập hợp, và sử dụng các tập hợp đó để thực thi các kiểm tra chặt chẽ tại thời gian biên dịch, bạn sẽ có khả năng thoải mái hơn với các tính năng nâng cao của TypeScript (nếu chưa thoải mái), cho phép bạn bắt được nhiều lỗi sớm hơn.

Mô hình tư duy này không hoàn hảo, nhưng nó hoạt động khá tốt ngay cả với một số tính năng phức tạp nhất của TypeScript.
