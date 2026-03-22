# GroceryApp

A shared grocery list app for iPhone and iPad, built with SwiftUI, Core Data, and CloudKit.

## Features

- **Smart Inventory Tracking** -- Set target quantities and restock thresholds. Items automatically appear in "Need to Buy" when they run low.
- **Real-Time Collaboration** -- Share your list with family or housemates via iCloud. Changes sync instantly across all devices.
- **Photo Attachments** -- Attach photos to items using the camera or photo library so everyone knows exactly what to get.
- **Category Organization** -- Sort and filter items by category (produce, dairy, meat, frozen, bakery, and more).
- **Freezer Tracking** -- Mark items stored in the freezer to keep track of what's frozen vs. refrigerated.
- **Quick Add** -- Add items instantly from the main screen without opening a form.
- **CSV Export** -- Export your full inventory to a spreadsheet for meal planning or budgeting.
- **Delete My Data** -- One-tap deletion of all local and iCloud data for privacy compliance.

## Architecture

- **SwiftUI** with `@Observable` pattern (iOS 17+)
- **Core Data** with `NSPersistentCloudKitContainer` for automatic CloudKit sync
- **Dual-store architecture** -- private store (owner's data) + shared store (collaborator's data)
- **CKShare-based collaboration** with `UICloudSharingController`
- **App Group container** for shared data storage

### Sync Contract

Every new item follows a 3-step pattern to ensure bidirectional sync:

1. `viewContext.assign(newItem, to: stack.storeForNewItems())`
2. `stack.saveContext()`
3. `stack.addToExistingShareIfNeeded(newItem)`

See `CoreDataStack.swift` for full documentation.

## Requirements

- iOS 17.0+ / iPadOS 17.0+ / macOS 14.0+
- Xcode 16+
- Apple Developer account (for CloudKit)

## Setup

1. Clone the repository
2. Open `GroceryApp.xcodeproj` in Xcode
3. Update the bundle identifier and signing team
4. Ensure the CloudKit container (`iCloud.com.briankinsella.GroceryApp`) and App Group (`group.com.briankinsella.GroceryApp`) are configured in your Apple Developer account
5. Build and run

## Project Structure

```
GroceryApp/
  CoreDataStack.swift        -- Data persistence and CloudKit sync (critical infrastructure)
  ContentView.swift           -- Main grocery list screen
  AddItemView.swift           -- New item creation form
  EditItemView.swift          -- Item editing with child context isolation
  CollaborationView.swift     -- Collaboration hub (members, invite, manage)
  CloudSharingController.swift -- UICloudSharingController wrappers
  SettingsView.swift          -- App settings, export, data deletion
  AdminConsoleView.swift      -- Diagnostics panel (hidden, tap version 7x)
  SyncMonitor.swift           -- CloudKit sync event monitoring
  GroceryItem+CoreData*       -- Core Data entity class and properties
  GroceryCategory.swift       -- Category enum
  FilterOption.swift          -- List filter options
  SortOption.swift            -- List sort options
```

## Privacy

- All data stored in the user's personal iCloud account
- No third-party analytics, advertising, or tracking
- No data sent to external servers
- [Privacy Policy](https://bdkinsella-rgb.github.io/GroceryApp/privacy-policy.html)

## Support

[Support Page](https://bdkinsella-rgb.github.io/GroceryApp/support.html) | [KindersDevelopment@icloud.com](mailto:KindersDevelopment@icloud.com)

## License

Copyright 2026 Kinders Development. All rights reserved.
