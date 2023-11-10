---
sidebar_position: 1
---

# Fundamental

Kenapa pakai React Query ?
React Query adalah tools yang memudahkan kita untuk memanage call api. Dengan React Query kita dapat lebih efesien dalam memaintenance code dan sangat mudah digunakan. Tidak hanya itu, React Query mempunyai performance yang baik sekali.

Beberapa Fitur yang terdapat pada React Query, diantaranya :

1. Menghandle Loading & Error State
2. Optimistic Update
3. Performance optimizations untuk pagination dan lazy loading data
4. Memoizing query results
5. Updating "out of date" data di background
6. Menghapus duplikat beberapa permintaan untuk data yang sama menjadi satu permintaan
7. Caching

## Install React Query

```js title="Terminal"
$ yarn add react-query
# atau
$ npm i react-query
```

## Inialisasi

```tsx title="App.tsx"
import { QueryClient, QueryClientProvider } from "react-query";

const queryClient = new QueryClient();

function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <Children />
    </QueryClientProvider>
  );
}
```

## Get Data

```tsx title="Products.tsx"
import { useQuery } from "react-query";
import axios from "axios";

type Product = {
  id: string;
  name: string;
  price: number;
  description: string;
  image: string;
};

function getProducts(): Promise<Product[]> {
  return axios.get("/items/products");
}

function Products() {
  const { data, error, isLoading, isFetching, isError } = useQuery<Product[]>(
    "products",
    getProducts
  );

  if (isLoading || isFetching) return <span>Loading....</span>;

  if (isError) return <span>Error: {error.message}</span>;

  return (
    <>
      {data?.map((item) => (
        <div>
          <h2>{item.name}</h2>
          <p>{item.description}</p>
          <p>{item.price}</p>
        </div>
      ))}
    </>
  );
}
```

Untuk mengambil data kita bisa menggunakan `useQuery(key, function_call_api)`

- key **( Harus Unique )**
- function_call_api ( Function untuk memanggil API )

useQuery mempunyai beberapa object balikannya yaitu :

- data ( Result )
- error ( error message akan muncul ketika data isError bernilai true)
- isError ( Ketika data gagal didapatkan )
- isLoading ( Ketika data sedang dalam proses pemanggilan )
- isFetching ( Ketika data sedang sedang dalam proses pemanggilan di backround )
- isSuccess ( Ketika data berhasil didapatkan )
