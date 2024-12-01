---
title: Header crashing with flatlist
tags:
  - bug
  - react-native
  - safe-area
---
<div style="display: flex; justify-content: space-between; align-items: center;"> <div style="text-align: center;"> <img src="headercrashbug.gif" alt="Before Bug Fix" style="width: 45%; height: auto;" /> <p>Before Bug Fix</p> </div> <div style="text-align: center;"> <img src="resolvedheadercrashbug.gif" alt="After Bug Fix" style="width: 45%; height: auto;" /> <p>After Bug Fix</p> </div> </div>

How to resolve above bug?

```tsx

import { useHeaderHeight } from "@react-navigation/elements";
import { useSafeAreaInsets } from "react-native-safe-area-context";

const NewChatScreen = () => {
  const insets = useSafeAreaInsets();
  const headerHeight = useHeaderHeight();

return(
	<FlashList
        data={filteredContacts}
        renderItem={renderItem}
        estimatedItemSize={56}
        contentContainerStyle={{
          paddingBottom: insets.bottom,
          paddingTop: headerHeight,
        }}
        ListEmptyComponent={EmptyContacts}
	  />
)
}
```

solution is to give padding, but not any hardcoded values, using insets and headerheight from react-navigation and safe-area-context


