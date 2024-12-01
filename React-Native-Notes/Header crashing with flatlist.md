---
title: Header crashing with flatlist
tags:
  - bug
  - react-native
  - safe-area
---
![[headercrashbug.gif]]

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

After:
![[resolvedheadercrashbug.gif]]