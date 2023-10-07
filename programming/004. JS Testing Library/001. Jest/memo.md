- snapshot
- describe, test記載方法
- axiosモック方法(1. jest.mock("axios")した上でaxios.mockImplementation)
- 名前exportしてるやつのMock化
```ts
// utils/s3Client.ts
export const getObjectReadStream = (key: string): stream.Readable => {}

// test.ts
import * as s3Client from '../utils/s3Client';

jest.spyOn(s3Client, 'getObjectReadStream').mockReturnValue(null);
```
- Default exportしてるやつのMock化
```ts
// dbutils.ts
const createDbConnection = async (): Promise<mysql.Connection> => {}

export default createDbConnection;

// test.ts
import * as createDbConnection from '../utils/dbUtils';

jest.spyOn(createDbConnection, 'default').mockReturnValue(new Promise((resolve) => resolve(mockMySqlConnection)));
```

- useEffectを`waitFor`関数で待機する。

- test.each の書き方