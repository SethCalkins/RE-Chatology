# Structure

```sql
CREATE TABLE IF NOT EXISTS account (
  id integer PRIMARY KEY AUTOINCREMENT,
  username text COLLATE NOCASE NOT NULL,
  service text NOT NULL,
  name text COLLATE NOCASE,
  name_update_date integer
);

CREATE INDEX IF NOT EXISTS username_index ON account (username);

CREATE TABLE IF NOT EXISTS chat (
  id integer PRIMARY KEY AUTOINCREMENT,
  path text UNIQUE NOT NULL,
  creation_date integer,
  modification_date integer,
  fts_docid integer UNIQUE,
  account_id integer,
  chat_type integer,
  group_chat_name text COLLATE NOCASE
);

CREATE INDEX IF NOT EXISTS account_id_modification_date_index ON chat (account_id, modification_date);

CREATE INDEX IF NOT EXISTS group_chat_name_index ON chat (group_chat_name);

CREATE INDEX IF NOT EXISTS chat_type_index ON chat (chat_type);

CREATE INDEX IF NOT EXISTS account_id_index ON chat (account_id);

CREATE INDEX IF NOT EXISTS modification_date_index ON chat (modification_date);

CREATE INDEX IF NOT EXISTS creation_date_index ON chat (creation_date);

CREATE VIRTUAL TABLE IF NOT EXISTS chat_content (
  content
) USING fts4();

CREATE TABLE IF NOT EXISTS chat_content_content (
  docid integer PRIMARY KEY,
  c0content
);

CREATE TABLE IF NOT EXISTS chat_content_docsize (
  docid integer PRIMARY KEY,
  size blob
);

CREATE TABLE IF NOT EXISTS chat_content_segdir (
  level integer,
  idx integer,
  start_block integer,
  leaves_end_block integer,
  end_block integer,
  root blob,
  PRIMARY KEY(level, idx)
);

CREATE TABLE IF NOT EXISTS chat_content_segments (
  blockid integer PRIMARY KEY,
  block blob
);

CREATE TABLE IF NOT EXISTS chat_content_stat (
  id integer PRIMARY KEY,
  value blob
);

CREATE TABLE IF NOT EXISTS groupchat_account (
  chat_id integer,
  account_id integer
);

CREATE INDEX IF NOT EXISTS groupchat_account_id_index ON groupchat_account (account_id);

CREATE INDEX IF NOT EXISTS groupchat_chat_id_index ON groupchat_account (chat_id);

CREATE TABLE IF NOT EXISTS metadata (
  "key" text UNIQUE NOT NULL,
  value integer
);
```


