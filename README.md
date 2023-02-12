# MFE

# anotações importantes

- Fluxo de navegação via callback
```
 const history =
    defaultHistory ||
    createMemoryHistory({
      initialEntries: [initialPath],
    });
  if (onNavigate) {
    // filho pro pai
    history.listen(onNavigate);
  }
  ReactDOM.render(<App history={history} />, el);

  return {
    onParentNavigate({ pathname: nextPathname }) {
      const { pathname } = history.location;
      // pai pro filho
      console.log("Container just navigated", nextPathname);
      if (pathname !== nextPathname) {
        history.push(nextPathname);
      }
    },
  };
};
```

- Fluxo de autenticação via callback
```
export default ({ onSignIn }) => {
  const ref = useRef(null);
  const history = useHistory();

  useEffect(() => {
    const { onParentNavigate } = mount(ref.current, {
      initialPath: history.location.pathname,
      onNavigate: ({ pathname: nextPathname }) => {
        const { pathname } = history.location;

        if (pathname !== nextPathname) {
          history.push(nextPathname);
        }
      },
      onSignIn,
    });

    history.listen(onParentNavigate);
  }, []);

  return <div ref={ref} />;
};
```
