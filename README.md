# decorators

> TypeScript React decorators.

## 安装

```bash
npm install @alibaba_inc/decorators
```

```typescript
import React, { useState, useEffect } from "react";
import type { HooksProps } from "@alibaba_inc/decorators";
import { State, Hooks, Get, Post, Perfect, Mount, match } from "@alibaba_inc/decorators";

type PerfectKey = "name" | "extract_nest_data";

InitPerfectData((meta_key: PerfectKey, data: any) => {
	if (meta_key) {
		match(meta_key, {
			name: async () => {
				data.name = data?.name ?? data?.nickname ?? data?.nick_name;
			},
			extract_nest_data: async () => {
				data = data?.data;
			},
			//...
		});
	}
	return data;
});

export default class Self extends React.PureComponent<{}, {}> {
	private uuid = "a2a556d7-48c3-11f0-acf2-00163e04cc20";

	@State private data: any;

	@Get("/user/info", { uuid: "@uuid" })
	public async get_user(@Perfect<PerfectKey>("@data", "name") _: any) {}

	private hooks_name;
	@Hooks("hooks_name")
	private HooksName({ data }: HooksProps<string, any>) {
		return data;
	}

	@Mount()
	async componentDidMount() {}

	render() {
		let This = this;
		return (
			<div>
				<div>
					hooks: <This.HooksName _this={this} />
				</div>
				<button
					onClick={() => {
						this.hooks_name = "this is decorators hooks";
					}}
				>
					click hooks
				</button>
				<div>this.data: {JSON.stringify(this.data)}</div>
			</div>
		);
	}
}
```
