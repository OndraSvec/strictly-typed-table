# strictly-typed-table

This repository includes a code snippet that highlights the power of `TypeScript` generics. In a few lines of code, we can create a `Table` component whose row items are determined by whatever columns we define in the component's props.

```
const Example = () => (
  <Table
    columns={['name', 'age', 'occupation']}
    rows={[
      { age: '30', name: 'Mike', occupation: 'Broker' },
      { age: '19', name: 'Tom', occupation: 'Personal Trainer' },
    ]}
  />
)

const Table = <TCols extends string[]>({ columns, rows }: TTable<TCols>) => {
  return (
    <table>
      <Head columns={columns} />
      <tbody>
        {rows.map((row) => (
          <Row columns={Object.values(row)} key={crypto.randomUUID()} />
        ))}
      </tbody>
    </table>
  )
}

const Head = ({ columns }: RowProps) => {
  return (
    <thead>
      <tr>
        {columns.map((column) => (
          <th key={crypto.randomUUID()} scope="col">
            {column}
          </th>
        ))}
      </tr>
    </thead>
  )
}

const Row = ({ columns }: RowProps) => {
  return (
    <tr>
      {columns[0] && <th scope="row">{columns[0]}</th>}
      {columns.slice(1).map((column) => (
        <td key={crypto.randomUUID()}>{column}</td>
      ))}
    </tr>
  )
}

type TTable<TCols extends string[]> = {
  columns: [...TCols]
  rows: TCols extends [] ? [] : Row<TCols>[]
}

type Row<TCols extends string[]> = ColumnsWithValues<TCols>

type ColumnsWithValues<TCols extends string[]> = {
  [Col in TCols[number]]: string
}

type RowProps = {
  columns: string[]
}
```
