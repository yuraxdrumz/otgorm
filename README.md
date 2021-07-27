# OpenTelemetry Package for the Gorm ORM V1 #

This package is meant to simplify wrapping Gorm requests to databases with OpenTelemetry Tracing Spans. The functionality within the package is as of OpenTelemetry-go v1.0.0-RC2 and is subject to change fairly rapidly as the standard is evolved.

This package is B.Y.O.E. (Bring Your Own Exporter)

## Prerequisites ##

Make sure you have the following:

- Go 1.16

### Example Usage ###

```golang
func InitGorm() {
	db, err = gorm.Open("postgres", tmpDb)
	handleErr(err)
	db.LogMode(true)
	// Register callbacks for GORM, while also passing in config Opts
	// Pass tracer from otel package to otgorm
	otgorm.RegisterCallbacks(db,
		otgorm.WithTracer(tp.Tracer("gorm")),
		otgorm.Query(true),
		otgorm.AllowRoot(true),
	)
}

func CreateUser(ctx context.Context, db *gorm.DB) {
	// alternatively you can use otgorm.SetSpanToGorm(ctx, db)
	db := otgorm.WithContext(ctx, db) 
	db.Create(&User{name: "foo"})
}
```

## License ##

The MIT License (MIT). Please see [License File](LICENSE) for more information.
