# Changelog

## [0.2.0] - 2021-07-12

Update to work with Solid 1.0 and streamline the interface with new directives.

### Added

- Update `createDraggable`, `createDroppable` and `createSortable` to provide
  a custom directive. These can be used with the Solid `use:___` syntactic sugar
  for common usage setup - no need to pass refs and props around:

```js
const MyComponent = (props) => {
  const draggable = createDraggable({ id: props.id });
  return <div use:draggable>Drag me!</div>;
};
```

If finer control needed, the underlying primitives are accessible by property
lookup on the directive::

```js
const MyComponent = (props) => {
  const draggable = createDraggable({ id: props.id });
  return (
    <div ref={draggable.ref}>
      <div {...draggable.dragActivators}>Drag Handle</div>
      Drag me!
    </div>
  );
};
```

- Automatically detect and register usage of `DragOverlay`. Add to
  `DragDropContext` state a `usingDragOverlay` property that can be checked.
- Return a new function `displace` as part of the `DragDropContext` that can be
  used to set the transform for a droppable or draggable. See usage in
  `createSortable` for example.

### Changed

- Update to work with Solid 1.0! This is a breaking change that requires
  updating peer dependency of Solid to 1.0 or greater.
- Refactor `isActiveDraggable` and `isActiveDroppable` to appear as resolved
  properties rather than functions to call (this is more consistent with the
  rest of interface). E.g. `draggable.isActiveDraggable()`
  -> `draggable.isActivateDraggable`.
- Refactor sensor interface to use event type rather than handler name. For
  example, make `createPointerSensor` use `pointerdown` rather than
  `onPointerDown` for activation. As part of this, add a `asHandlers` option to
  `draggableActivators` to control whether to return object with `on___` form or
  not (default is `false`).
- Rename occurrences of `translate` to `transform`, which feels a more
  appropriate name. E.g. `translateStyle` should now be `transformStyle` and the
  `translate` property returned from `create___` calls should be called
  `transform`.
- Assume valid `transform` value is always present. In particular,
  `transformStyle` no longer checks for a `null` transform value.
- Improve vertical sort algorithm to better handle arbitrary gaps (such as those
  created by the CSS gap property on flexbox). As part of this, remove the now
  redundant `outerHeight` and `outerWidth` properties from the layout data
  returned by `elementLayout`.
- Update `README.md` to reflect simpler directive interface for draggables and
  droppables.

### Removed

- Remove support for explicitly managing droppable `disabled` state. The current
  implementation feels limiting and potentially confusing. For example, it
  prevents detecting when a draggable is over a disabled droppable and styling
  it appropriately. Note that a similar effect to `disabled` can be achieved by
  determining drop suitability in `onDragEnd`.

## [0.1.2] - 2021-07-03

Simplify releasing.

### Added

- Use [release-it](https://github.com/release-it/release-it) to simplify
  performing a release.
- Add keywords to `package.json` for easier discoverability of package.
- Add default publish configuration to `package.json`.

## [0.1.1] - 2021-06-22

Initial release supporting plain drag and drop as well as an early preset for
sortable lists.

### Added

- Add `createDraggable` to easily integrate drag behaviour into components, with
  the component maintaining control over how it looks and behaves.
- Add `createDroppable` to register and manage droppable areas.
- Support conditionally enabling and disabling droppables via a `disabled`
  option on `createDroppable`.
- Support dragging representations of a draggable through the use of a
  `DragOverlay` (which can be removed from the normal document flow).
- Support using sensors to detect and manage drag starts.
- Add `createPointerSensor` as the default sensor, and wrap for convenience in a
  `<DragDropSensors/>` component.
- Support customising collision detection algorithms and provide two initial
  ones for common usage (`mostIntersectingLayout` and `closestLayoutCenter`).
- Add sortable list primitives (`SortableContext` and `createSortable`) for drag
  and drop vertical list reordering.
- Support using multiple (or nested) `DragDropContext` for containers isolated
  from each other.

[unreleased]: https://github.com/thisbeyond/solid-dnd/compare/0.2.0...HEAD
[0.2.0]: https://github.com/thisbeyond/solid-dnd/compare/0.1.2...0.2.0
[0.1.2]: https://github.com/thisbeyond/solid-dnd/compare/0.1.1...0.1.2
[0.1.1]: https://github.com/thisbeyond/solid-dnd/releases/tag/0.1.1
