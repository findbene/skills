---
name: frontend-implementation
description: "High-performance frontend application implementation with React, TypeScript, Vite, performance optimization, accessibility, and PWA patterns. Use this skill any time a frontend application needs to be built, React components need to be implemented, performance optimization is required, or frontend architecture needs to be established. Trigger immediately on: \"React\", \"TypeScript frontend\", \"build this component\", \"frontend implementation\", \"React component\", \"performance optimization\", \"PWA\", \"bundle optimization\", \"lazy loading\", \"code splitting\", \"Vite\", \"webpack\", \"React hooks\", \"frontend build\". If someone says \"implement this frontend feature\" or \"build this React component\" this skill MUST trigger."
---

# Frontend Implementation

Build production-ready frontend applications with proper architecture, state management, and performance optimization.

## Architecture Patterns

### Project Structure
```
src/
├── app/                    # Next.js App Router pages
├── components/
│   ├── ui/                 # Primitive UI components
│   ├── forms/              # Form components
│   ├── layouts/            # Layout components
│   └── features/           # Feature-specific components
├── hooks/                  # Custom React hooks
├── lib/                    # Utility functions
├── services/               # API services
├── stores/                 # State management
├── types/                  # TypeScript types
└── styles/                 # Global styles
```

### Feature-Based Components
```tsx
// components/features/ProductCard/ProductCard.tsx
import { useProductCard } from './useProductCard';
import type { ProductCardProps } from './ProductCard.types';

export function ProductCard({ product, onAddToCart }: ProductCardProps) {
  const { isHovered, handlers, formattedPrice } = useProductCard(product);
  return (
    <article className="group relative rounded-xl overflow-hidden" {...handlers}>
      <div className="aspect-square overflow-hidden">
        <img src={product.image} alt={product.name}
          className={cn('w-full h-full object-cover transition-transform duration-300', isHovered && 'scale-105')} />
      </div>
      <div className="p-4">
        <h3 className="font-semibold text-lg">{product.name}</h3>
        <p className="text-muted-foreground">{formattedPrice}</p>
        <Button onClick={() => onAddToCart(product)} className="mt-2 w-full">Add to Cart</Button>
      </div>
    </article>
  );
}
```

## State Management

### React Context for Global State
Use `useReducer` + Context for complex local state. Define typed actions with discriminated unions for type safety.

### Server State with React Query
```tsx
export function useProducts(filters?: ProductFilters) {
  return useQuery({
    queryKey: ['products', filters],
    queryFn: () => productService.getProducts(filters),
    staleTime: 5 * 60 * 1000,
  });
}
```

## Data Fetching

### Server Components (Next.js)
```tsx
async function ProductList({ category }: { category?: string }) {
  const products = await fetch(`${process.env.API_URL}/products?category=${category}`,
    { next: { revalidate: 60 } }).then(res => res.json());
  return <ProductGrid products={products} />;
}
```

### Client-Side with Debounce
Use `useDebounce` for search inputs, `useTransition` for non-urgent updates.

## Form Handling (React Hook Form + Zod)

```tsx
const schema = z.object({
  email: z.string().email(),
  firstName: z.string().min(1),
});
type FormData = z.infer<typeof schema>;

const { register, handleSubmit, formState: { errors } } = useForm<FormData>({
  resolver: zodResolver(schema)
});
```

## Performance Optimization

- **Images**: Use Next.js `Image` with `placeholder="blur"` and `priority` for above-fold
- **Code splitting**: `dynamic(() => import(...), { ssr: false })` for heavy client-only components
- **Memoization**: `useMemo` for expensive computations, `useCallback` for stable callbacks, `memo` for pure components
- **Bundle**: Direct imports instead of barrel files; dynamic imports for heavy libraries

## Error Handling

- Use Next.js `error.tsx` for route-level error boundaries
- Create typed `APIError` classes with status codes
- Always show user-friendly error messages with retry actions

## Accessibility

- Use semantic HTML and ARIA attributes
- Implement keyboard navigation with arrow key handlers
- Focus management for modals and dialogs
